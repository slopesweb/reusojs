# Claude Code Guidance

Read `AGENTS.md`, `PROJECT_CONTEXT.md`, and `.specify/memory/constitution.md` before work.

GitHub Spec Kit is mandatory for non-trivial changes. Keep repository artifacts in English. Directus Studio is private infrastructure, not the customer admin. Customer applications use the product-owned API.

Organization is the tenant boundary; Workspaces group Sites; Sites own content and API keys. Resolve authorization from authenticated principals and credentials, never untrusted request identifiers. PostgreSQL is the source of truth. Keep Directus behind an adapter and use a modular monolith unless an approved ADR changes that decision.

Resolve material ambiguities through Spec Kit before implementation. Add negative tests for cross-tenant, cross-workspace, cross-site, IDOR, privilege escalation, API-key scope, and media access. Report only validations actually executed.
