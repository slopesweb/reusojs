# Agent Instructions

Read `PROJECT_CONTEXT.md` and `.specify/memory/constitution.md` before work.

## Mandatory workflow
- Use GitHub Spec Kit for all non-trivial features and architectural changes.
- Follow the active feature's spec, plan, tasks, checklists, and constitution.
- Resolve material requirement and security ambiguities before implementation.
- Run analysis and validation gates before implementation; run convergence after.
- Work in small, reviewable changes and report exact commands and outcomes.

## Project constraints
- Keep repository artifacts in English.
- Preserve Directus Studio / customer product separation.
- Preserve Organization → Workspace → Site ownership and invariants.
- PostgreSQL is the source of truth; Supabase remains optional.
- Use a modular monolith unless an ADR approves otherwise.
- Keep Directus behind an application-owned adapter.
- Never trust client-supplied tenant/site identifiers for authorization.
- Add cross-tenant and cross-site negative tests for every data-access capability.
- Never log secrets, raw API keys, or sensitive tenant data.

## Implementation discipline
- Validate input at trust boundaries.
- Enforce authorization server-side, not only in UI.
- Use database constraints for ownership and uniqueness where possible.
- Use migrations; document rollback or forward recovery.
- Keep API contracts versioned and documented.
- Add structured logs, correlation IDs, and audit events for sensitive operations.
- Do not add dependencies without rationale.
- Never claim an unexecuted check passed.

## Git
- Do not force-push or rewrite shared history.
- Keep commits focused and descriptive.
- Record checkpoint SHAs for milestones.
