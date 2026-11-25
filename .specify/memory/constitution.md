# reusojs Constitution

## Core Principles

### I. Tenant Isolation Is a Security Boundary
Organization is the primary tenant boundary. Every tenant-owned record has an unambiguous ownership path to one Organization. Workspace and Site access are checked on every request. Cross-tenant access is denied by default. Client identifiers never prove authorization.

### II. Directus Is Infrastructure, Not the Product Contract
Directus Studio is private and reserved for internal operators. Customer applications use product-owned applications and API contracts. Directus is accessed through an adapter; its collections, permissions, and endpoints must not become an accidental public API.

### III. API-First, Contract-Driven Design
Capabilities use documented, versioned contracts. The initial public REST API uses `/v1`. Validate inputs and outputs at trust boundaries. Breaking changes require explicit versioning and migration notes.

### IV. Security-First Defaults
Authentication, authorization, tenant resolution, validation, secrets handling, rate limiting, and audit policy are server-side. API keys are site-scoped, hashed at rest, shown once, revocable, and scope-limited. Least privilege is the default.

### V. PostgreSQL Is the Source of Truth
PostgreSQL is mandatory. Supabase may be optional infrastructure but is not a required dependency. Database constraints and migrations preserve domain invariants.

### VI. Test Boundaries, Especially Denials
Every tenant-data feature includes tests for allowed and denied cross-tenant/site access. IDOR, privilege escalation, API-key scope bypass, and unauthorized media access are required test categories. TDD is preferred for business and security-sensitive rules.

### VII. Modular Monolith Before Distributed Complexity
Start with a modular monolith and explicit application boundaries. Microservices and per-tenant databases require an ADR demonstrating concrete need and operational readiness.

### VIII. Observable and Auditable Operations
Use structured logs, correlation IDs, metrics, and traces where appropriate. Never log credentials or raw API keys. Security-sensitive administrative actions produce attributable audit records.

### IX. Internationalization From Day One
Externalize user-facing strings and design for pt-BR and en-US. Include pseudo-localization and RTL readiness in design and test strategy. Content translation workflows are outside initial scope.

### X. Small, Reviewable, Reproducible Changes
Use Spec Kit for non-trivial changes. Keep specs, plans, tasks, ADRs, and validation synchronized. Changes must be reviewable and tied to commit SHAs. Do not claim unexecuted checks passed.

## Technical Constraints
- PostgreSQL is the required system of record.
- Directus is accessed through an application-owned adapter.
- Product API is the customer-facing contract.
- Organization → Workspace → Site ownership is mandatory.
- Each Site owns content, categories, tags, media, settings, and API keys.
- Initial architecture is a modular monolith.
- Public API routes are versioned under `/v1`.
- Initial post statuses are `draft` and `published`.

## Development Workflow
Use Spec Kit: constitution → specify → clarify → plan → checklist → tasks → analyze → implement → converge. Resolve material ambiguity before implementation. Database changes use migrations and document rollback or forward recovery. Every feature includes automated tests and recorded validation. Security-sensitive changes require explicit threat/abuse review.

## Governance
This constitution supersedes conflicting convenience or undocumented conventions. Amendments require rationale, review of affected specs and ADRs, and a recorded version/date change. Feature plans demonstrate compliance.

**Version**: 1.0.0 | **Ratified**: 2026-11-25 | **Last Amended**: 2026-11-25
