# Product Surfaces

## Directus Studio (GodMode)
Private infrastructure and data operations interface for authorized internal operators. Not tenant-facing. Access is restricted, separately authenticated, and audited.

## SuperAdmin
Product-owned platform console for Organizations, platform configuration, support workflows, governance, and audit visibility. Uses Product API; it does not provide unrestricted database access.

## Customer Admin
Organization-scoped administration for Workspaces, Sites, invitations, memberships, permissions, site settings, media, and API keys according to grants.

## Content User
Site-scoped operations for posts, categories, tags, and media. UI visibility is not a security control; every operation is authorized server-side.

## Boundary rule
Never repurpose Directus Studio as the commercial customer admin. Product surfaces remain independent of Directus UI and API internals.
