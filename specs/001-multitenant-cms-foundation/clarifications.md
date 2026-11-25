# Clarification Register

**Feature**: [001-multitenant-cms-foundation](spec.md)  
**Status**: Open — do not generate implementation tasks until blocking questions are resolved.

This register captures decisions that the Spec Kit clarification step must settle. Items marked **Blocking** affect observable requirements or security behavior. Items marked **Planning** are technology/architecture choices and belong in `plan.md`, not in the technology-agnostic feature specification.

## Blocking requirement clarifications

| ID | Question | Why it matters | Safe baseline to evaluate |
|---|---|---|---|
| C-001 | What is the human identity source for the first slice: existing identity provider, product-managed accounts, or an explicitly temporary development identity? | Defines the trust boundary for memberships and authenticated content management. | Keep identity provider abstract in the spec; select the actual provider during planning. |
| C-002 | How is the first Organization and its initial owner provisioned? | The feature needs a bootstrap path without creating an unaudited privilege-escalation route. | A platform operator creates the Organization and assigns its first owner through an audited operation. |
| C-003 | Which minimum permissions are required for the vertical slice, and which actors may manage posts versus manage API keys? | Prevents accidental privilege equivalence between organization owners, site administrators, editors, and read-only users. | Separate content-management permission from API-key administration; finalize a minimal role/permission matrix before implementation. |
| C-004 | Are human users allowed to read drafts through the management surface while external API keys can read published content only? | Clarifies the difference between editorial access and public delivery. | Yes, subject to explicit site content-read permission; machine read keys see published content only. |
| C-005 | Should unauthorized resource lookups return 404 (conceal existence) or 403 (explicit denial) when the principal is authenticated but lacks access? | Affects resource enumeration resistance and API consistency. | Prefer a consistent non-enumerating response for resources outside the caller's authorized scope. |
| C-006 | What are the deletion semantics for Posts in this first slice: hard delete or recoverable soft delete? | Affects auditability, uniqueness, API behavior, and retention. | Defer irreversible deletion policy until the user explicitly chooses; do not silently assume. |
| C-007 | What slug normalization rules apply (case, accents, Unicode, punctuation), and does a deleted post release its slug? | Required for deterministic uniqueness and concurrent-write behavior. | Define canonical normalization and uniqueness at the Site boundary before schema design. |

## Planning decisions (not feature-spec blockers)

| ID | Decision | Constraint |
|---|---|---|
| P-001 | Backend runtime/framework | Must support the modular-monolith boundary, explicit authorization, PostgreSQL, and Directus adapter. |
| P-002 | Customer frontend framework | Must remain independent of Directus Studio. |
| P-003 | Directus version, deployment, and adapter strategy | Directus must remain behind the application-owned boundary. |
| P-004 | PostgreSQL access and RLS strategy | Verify the actual database role used by Directus; RLS is defense in depth, not a substitute for application authorization. |
| P-005 | API key format and transport | Secrets must be random, hash-only at rest, one-time displayed, site-scoped, revocable, and scope-limited. |
| P-006 | Media storage and delivery | Media is out of scope for this feature; do not couple the post model to a vendor-specific storage implementation. |
| P-007 | Local development, CI, staging, and production topology | Staging must reproduce production authorization and database-role behavior. |

## Exit criteria for clarification

- All **Blocking** questions have explicit answers or an explicitly accepted safe default.
- The answers are reflected in `spec.md`; no contradictory assumptions remain.
- Technology choices are recorded in `plan.md` and accepted ADRs.
- No implementation tasks are generated while a blocking security or lifecycle question remains unresolved.
