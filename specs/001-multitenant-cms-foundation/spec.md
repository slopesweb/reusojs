# Feature Specification: Multi-Tenant CMS Foundation

**Feature Branch**: `001-multitenant-cms-foundation`

**Created**: 2026-11-25

**Status**: Draft — pending clarification and approval

**Input**: Establish the first independently demonstrable CMS vertical slice for reusojs: organizations contain workspaces, workspaces contain independent sites, authorized users manage site-scoped posts, and external consumers retrieve published posts through a product-owned REST API. The feature must prove tenant and site isolation from its first implementation.

## User Scenarios & Testing

### User Story 1 — Provision isolated organizations and sites (Priority: P1)

A platform operator provisions two independent customer organizations. Each organization receives a default workspace and can own multiple sites. A site belongs to exactly one workspace and organization.

**Why this priority**: Every later CMS capability depends on a correct tenant ownership path. This story establishes the isolation boundary that the vertical slice must prove.

**Independent Test**: Provision two organizations, each with a default workspace and at least one site. Verify ownership relationships and prove that a site cannot be attached to another organization.

**Acceptance Scenarios**:

1. **Given** no customer organization exists, **When** an authorized platform operator provisions an organization, **Then** the system creates the organization and exactly one designated default workspace.
2. **Given** an organization exists, **When** an authorized operator creates another workspace and multiple sites, **Then** each site belongs to exactly one workspace and the organization is derived through that workspace.
3. **Given** a site belongs to Organization A, **When** an actor attempts to reassign it to Organization B without an explicitly authorized ownership-transfer workflow, **Then** the operation is rejected and the original ownership remains unchanged.
4. **Given** two organizations exist, **When** an actor operating in Organization A requests Organization B's organization, workspace, or site data without authorization, **Then** the system denies access without disclosing protected data.

### User Story 2 — Manage posts within an authorized site (Priority: P1)

An authenticated content manager selects a site they are authorized to manage and creates, lists, reads, updates, and deletes posts belonging to that site.

**Why this priority**: Post management is the first tangible CMS capability and validates that content ownership is enforced below the user interface.

**Independent Test**: Using two organizations and at least two sites, create and manage posts in one site. Verify that posts are visible only within their owning site and authorized context.

**Acceptance Scenarios**:

1. **Given** an authenticated user has content-management permission for Site A, **When** they create a post with a valid title and slug, **Then** the post is stored as owned by Site A and defaults to `draft` unless an explicitly permitted status is supplied.
2. **Given** a user is authorized for Site A, **When** they list or read posts in Site A, **Then** only posts owned by Site A are returned.
3. **Given** a post belongs to Site A, **When** an authorized Site A content manager updates or deletes it, **Then** the change applies only to that post and its Site A ownership is preserved.
4. **Given** a post slug already exists in Site A, **When** another post is created or renamed with the same slug in Site A, **Then** the operation is rejected with a validation conflict.
5. **Given** the same slug is unused in Site B, **When** an authorized user creates a post with that slug in Site B, **Then** creation succeeds because slug uniqueness is scoped to a site.
6. **Given** a user has no permission for Site B, **When** they guess a Site B post identifier and attempt to read, update, or delete it, **Then** the system denies the request without revealing whether the post exists.

### User Story 3 — Deliver published content through a site-scoped API credential (Priority: P1)

An organization administrator creates an API key for a specific site. An external consumer uses that credential to retrieve published content from that site through the product-owned REST API.

**Why this priority**: This proves the headless CMS value proposition and validates machine-credential isolation independently of human sessions.

**Independent Test**: Create two sites and distinct API keys. Retrieve published posts with each key and verify that neither credential can access the other site's content or management operations outside its granted scope.

**Acceptance Scenarios**:

1. **Given** an authorized administrator manages Site A, **When** they create a read-only API key for Site A, **Then** the system returns the secret once and stores only a one-way cryptographic hash plus a non-secret lookup identifier.
2. **Given** a valid, active Site A read key, **When** an external consumer requests published posts through the product API, **Then** only published posts owned by Site A are returned.
3. **Given** a Site A API key, **When** a consumer supplies a Site B identifier or attempts to retrieve a Site B post, **Then** the request is denied and no Site B data is disclosed.
4. **Given** a key has been revoked or expired, **When** it is used, **Then** the API rejects it.
5. **Given** a read-only API key, **When** it is used for a content mutation, **Then** the API denies the operation.
6. **Given** an API key secret has already been displayed at creation, **When** an administrator later views the key record, **Then** the original secret cannot be retrieved.

### User Story 4 — Enforce the product and infrastructure boundary (Priority: P1)

A tenant user interacts only with product-owned surfaces and APIs. Directus Studio remains a private internal infrastructure interface.

**Why this priority**: The product must not expose Directus as its customer-facing admin experience or public API contract.

**Independent Test**: Attempt to access Directus Studio using tenant identities and verify that the customer API contract does not expose Directus-specific endpoints or credentials.

**Acceptance Scenarios**:

1. **Given** a tenant user has valid product credentials, **When** they attempt to access the private Directus Studio, **Then** access is denied.
2. **Given** an external API consumer has a valid site API key, **When** they use the documented product REST API, **Then** the response follows the product contract and does not require Directus credentials or Directus endpoint knowledge.
3. **Given** an internal operator performs a privileged platform operation, **When** the operation succeeds or is denied, **Then** an attributable audit event is recorded without storing secrets.

## Edge Cases

- A request omits its active organization or site context.
- A user belongs to multiple organizations and presents a resource identifier from the wrong active organization.
- A workspace is deleted while it still contains sites.
- An organization is suspended while sessions or API keys remain active.
- A site is archived or disabled while API keys remain active.
- Two concurrent requests attempt to create the same slug within one site.
- A post is changed from draft to published or from published to draft.
- A read API key attempts to access a draft post.
- A key is revoked between authentication and a subsequent request.
- A request attempts to override credential-derived site context using query, path, or body parameters.
- A media or featured-image reference points to an asset owned by another site (media management is not implemented in this feature; such references must not bypass ownership validation).
- Directus is unavailable or returns an error; the product API must not leak internal credentials, stack traces, or infrastructure details.
- A privileged Directus database role bypasses PostgreSQL RLS; application-level authorization must still deny unauthorized access.

## Requirements

### Functional Requirements

- **FR-001**: The system MUST represent a platform containing multiple Organizations.
- **FR-002**: The system MUST create a designated default Workspace for every Organization.
- **FR-003**: The system MUST support multiple Workspaces per Organization.
- **FR-004**: The system MUST support multiple Sites per Workspace.
- **FR-005**: Every Workspace MUST belong to exactly one Organization.
- **FR-006**: Every Site MUST belong to exactly one Workspace; its Organization ownership MUST be derived through that Workspace.
- **FR-007**: The system MUST support users participating in multiple Organizations through explicit membership relationships.
- **FR-008**: The system MUST authorize organization, workspace, site, and content operations server-side according to the authenticated principal and granted permissions.
- **FR-009**: The system MUST NOT treat client-supplied organization, workspace, or site identifiers as proof of authorization.
- **FR-010**: The system MUST provide create, list, read, update, and delete operations for posts within an authorized site.
- **FR-011**: Every post MUST belong to exactly one Site.
- **FR-012**: Every post MUST have a UUID, title, site-scoped slug, status, and creation/update timestamps.
- **FR-013**: The system MUST support at least `draft` and `published` post statuses.
- **FR-014**: A post slug MUST be unique within its Site; the same slug MAY exist in another Site.
- **FR-015**: The system MUST distinguish human-user authentication from machine API-key authentication.
- **FR-016**: The system MUST allow an authorized administrator to create and revoke API keys scoped to one Site.
- **FR-017**: The system MUST store API key secrets only as one-way cryptographic hashes and MUST show a newly generated secret only once.
- **FR-018**: API keys MUST support explicit scopes and MUST reject operations outside those scopes.
- **FR-019**: The system MUST reject revoked and expired API keys.
- **FR-020**: The product REST API MUST provide a versioned contract under `/v1`.
- **FR-021**: The product REST API MUST allow a valid site-scoped read credential to retrieve published posts from its own Site.
- **FR-022**: Public read operations MUST NOT expose draft posts through read-only external credentials.
- **FR-023**: The system MUST prevent cross-organization and unauthorized cross-site access for reads and mutations, including access by guessed resource identifiers.
- **FR-024**: The system MUST keep Directus Studio inaccessible to tenant identities.
- **FR-025**: The product API MUST own its request/response contract and MUST NOT require external consumers to use Directus credentials or Directus-specific endpoints.
- **FR-026**: Security-sensitive administrative actions and API-key lifecycle operations MUST produce attributable audit events.
- **FR-027**: Logs and error responses MUST NOT expose raw API keys, passwords, private infrastructure credentials, or sensitive internal stack traces.
- **FR-028**: The system MUST return consistent, documented errors for authentication failures, authorization failures, validation conflicts, and missing resources without enabling resource enumeration.

### Key Entities

- **Organization**: Primary tenant boundary; owns Workspaces and memberships.
- **Workspace**: Organizational grouping belonging to one Organization; one is designated default per Organization.
- **Site**: Independent content and API boundary belonging to one Workspace.
- **User**: Human identity that may participate in multiple Organizations.
- **Membership**: Relationship between User and Organization, including status and granted organization-level access.
- **Workspace Membership**: Contextual access relationship for a User within a Workspace.
- **Role / Permission**: Named grants used to authorize actions; exact role matrix is subject to clarification.
- **Post**: Content record owned by exactly one Site.
- **API Key**: Machine credential owned by one Site, with a secret hash, scopes, lifecycle state, and optional expiry.
- **Audit Event**: Attributable record of a security-sensitive or administrative action.

### API Surface (Proposed)

The following paths describe capability intent; exact methods, payloads, pagination, filtering, and error schemas are to be finalized in the API contract during planning.

- `GET /v1/posts`
- `GET /v1/posts/{id}`
- `POST /v1/posts`
- `PATCH /v1/posts/{id}`
- `DELETE /v1/posts/{id}`

The site context for machine requests is derived from the authenticated API key. Human management requests derive it from verified session context and authorization grants. A request-supplied `site_id` never overrides the authenticated context.

## Success Criteria

### Measurable Outcomes

- **SC-001**: A test environment can provision two Organizations, each with a default Workspace and at least two Sites, with all ownership constraints satisfied.
- **SC-002**: An authorized content manager can create, list, read, update, and delete a post in an authorized Site through the product contract.
- **SC-003**: A post created in one Site is never returned by list or read operations in another Site.
- **SC-004**: Automated negative tests demonstrate that cross-organization and unauthorized cross-site read and mutation attempts are denied, including IDOR attempts.
- **SC-005**: A read-only API key retrieves published posts only from its owning Site and cannot mutate content.
- **SC-006**: Revoked and expired API keys are rejected on the next authentication attempt.
- **SC-007**: Automated tests verify that Directus Studio cannot be accessed using tenant identities.
- **SC-008**: All privileged organization/site provisioning and API-key lifecycle actions in the feature produce attributable audit events.
- **SC-009**: No test or documented flow requires a customer to use Directus credentials or Directus endpoints.
- **SC-010**: The security test suite includes at least one negative case for each mandatory isolation boundary: Organization, Workspace, Site, resource ID, and API-key scope.

## Assumptions

- PostgreSQL is the system of record and the initial deployment uses a shared database with logical tenant isolation.
- Directus is used behind an application-owned adapter; its Studio remains a private operator surface.
- The first implementation is a modular monolith.
- The public REST API is product-owned and versioned under `/v1`.
- A Site is the ownership boundary for posts and machine API credentials.
- The initial post statuses are `draft` and `published`.
- Categories, tags, media management, customer UI, billing, comments, editorial revisions, multilingual content variants, and advanced scheduling are outside this first vertical slice and may be specified separately.
- This feature validates tenant and site isolation with a minimal set of membership and permission capabilities; the complete organization role matrix is not assumed here.
- The exact frontend/backend languages, frameworks, identity provider, Directus version, storage provider, deployment topology, and RLS configuration are implementation decisions for clarification and planning, not requirements of this technology-agnostic specification.

## Out of Scope

- A full SuperAdmin user interface.
- A complete customer Admin or Content User interface.
- Billing, subscriptions, plans, and usage metering.
- Category and tag management.
- Media upload, transformations, and delivery.
- Editorial review, revision history, comments, and advanced scheduling.
- Multilingual content variants.
- Page builder or visual site builder.
- Per-customer databases or schemas.
- Microservices.
