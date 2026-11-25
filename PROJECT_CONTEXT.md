# Project Context

## Identity and purpose
- Technical codename: `reusojs` (temporary, repository identity).
- Product: Directus-first, multi-tenant headless CMS with an application-owned REST API.
- Repository: `slopesweb/reusojs`.
- Status: architecture/specification bootstrap; runtime implementation has not started.
- Public brand remains undecided and configurable.

Build a focused proof of concept validating reusable SaaS foundations through site-scoped post management and content delivery. This is not a full general-purpose CMS.

## Product surfaces
1. **Directus Studio (GodMode):** private infrastructure operations surface, never the customer-facing application.
2. **SuperAdmin:** product-owned global platform administration.
3. **Customer Admin:** organization, workspace, site, membership, settings, and API-key administration.
4. **Content User:** site-scoped content operations.

Directus is infrastructure behind an adapter, not the public product contract.

## Domain hierarchy
`Platform → Organization → Workspace → Site → Content`
- Organization is the primary tenant isolation boundary.
- Every Organization has at least one default Workspace; it may have multiple Workspaces.
- Workspace groups sites and teams; it is not an independent tenant boundary.
- Workspace belongs to exactly one Organization and contains multiple Sites.
- Site belongs to exactly one Workspace and therefore one Organization.
- Posts, categories, tags, media, settings, and API keys belong directly to a Site.
- Users may participate in multiple Organizations. Membership and Workspace Membership express contextual access.

## Non-negotiable invariants
- Never trust client-supplied tenant, organization, workspace, or site IDs as authorization evidence.
- Resolve context from authenticated human principals or machine credentials; authorize every operation server-side.
- Resource IDs alone never authorize access (prevent IDOR).
- Slugs are unique within a Site.
- API keys are site-scoped, cryptographically random, hash-only at rest, shown once, individually revocable, scope-limited, optionally expiring, and auditable.
- API keys do not replace human authentication.
- SuperAdmin operations are authorized and audited; no unrestricted database access.
- Directus Studio is private and inaccessible to customers.
- PostgreSQL is the source of truth. Supabase is optional infrastructure, never mandatory.

## Architecture
Start with a modular monolith:
`SuperAdmin/Customer Apps/External Consumers → Product API/BFF → Application Services → Directus Adapter → Directus + PostgreSQL`

The Product API owns auth, authorization, tenant resolution, validation, business rules, response contracts, rate limits, and audit policy. Do not expose Directus endpoints as the public contract.

## Initial CMS scope
- Post CRUD and listing; statuses `draft` and `published`.
- Categories and tags with site-scoped associations.
- Media upload/listing and protected access.
- Site settings.
- Site-scoped API keys.
- Versioned REST API under `/v1`.

Proposed Post fields: UUID, site_id, title, slug, excerpt, content, featured_image reference, status, author_id, published_at, created_at, updated_at.

Out of initial scope: editorial revisions, multilingual content variants, advanced scheduling, comments, page builder, microservices, per-customer databases/schemas.

## Engineering standards
- GitHub Spec Kit is mandatory for non-trivial development.
- English-only repository docs, specs, comments, identifiers, and technical artifacts.
- API-first, security-first, test-first where appropriate, observable by design.
- i18n foundations from day one: pt-BR, en-US, pseudo-locale, RTL readiness.
- PostgreSQL mandatory as system of record.
- Cross-tenant and cross-site negative tests are mandatory.
- Document architectural changes as ADRs.
- No completion claim without executed tests and validation evidence.
- Record checkpoints and commit SHAs; do not promote past failed gates.

## Initial definition of done
1. SuperAdmin creates at least two isolated Organizations.
2. Each Organization has Workspaces and multiple Sites.
3. Admins invite users and assign different permissions.
4. Each Site manages its own posts, categories, tags, and media.
5. Each Site has independent API keys.
6. External consumers read published content through Product REST API.
7. Cross-tenant and unauthorized cross-site access is denied.
8. Customers cannot access Directus Studio.
9. Administrative operations are auditable.
10. Automated tests cover tenant isolation, IDOR, privilege escalation, key scopes, and media authorization.

## Open decisions
Spec Kit clarification/planning must resolve frontend framework, backend runtime, Directus version/deployment, adapter strategy, identity/auth implementation, media storage, tenancy enforcement, and deployment topology. Do not silently assume them.
