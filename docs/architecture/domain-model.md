# Domain Model

## Hierarchy
`Platform → Organization → Workspace → Site → Content`

## Entities
- Platform: global product scope and operational governance.
- Organization: tenant boundary and ownership aggregate.
- Workspace: organizational grouping; each Organization has a default Workspace.
- Site: independent content destination and API boundary.
- User: human identity that may participate in multiple Organizations.
- Membership: User-to-Organization relationship and organization role assignments.
- Workspace Membership: contextual access to a Workspace and its resources.
- Role / Permission: permission bundles and granular capabilities; detail in identity/access specification.
- Invitation: controlled onboarding into an Organization or Workspace.
- Post, Category, Tag: site-owned content and classifications.
- Media: site-owned asset metadata and storage reference.
- Site Settings: site-specific configuration.
- API Key: site-scoped machine credential with scopes, expiry, revocation, usage metadata.
- Audit Log: attributable record of security-sensitive and administrative actions.

## Ownership rules
1. Each Organization has one or more Workspaces and exactly one designated default.
2. Each Workspace belongs to exactly one Organization.
3. Each Site belongs to exactly one Workspace; Organization is derived through Workspace.
4. Each content object belongs directly to exactly one Site.
5. Each API Key belongs directly to exactly one Site.
6. A User may join multiple Organizations.
7. Workspace/site access requires active Organization membership and appropriate permissions.

## Initial Post fields
- `id`: UUID.
- `site_id`: required owning Site.
- `title`: required.
- `slug`: normalized and unique within Site.
- `excerpt`: optional summary.
- `content`: representation to be decided in planning.
- `featured_image_id`: optional site-owned Media reference.
- `status`: `draft` or `published`.
- `author_id`: authenticated human author where applicable.
- `published_at`: nullable timestamp.
- `created_at`, `updated_at`: timestamps.

Categories and Tags are site-scoped. Associations may never cross Site boundaries.

## Authorization
Every request establishes authenticated principal, active Organization membership or machine credential, allowed Workspace/Site context, action permission, and resource ownership before data access.

## Database strategy
Use a shared PostgreSQL deployment initially; no database/schema per customer. Enforce tenant ownership in application services and database constraints. Evaluate RLS only after verifying the actual Directus connection role and session behavior: privileged credentials may bypass policies. Never claim RLS protection without integration tests using production-equivalent roles.
