# Security Baseline

## Threat priorities
- Cross-tenant disclosure or mutation.
- Cross-workspace/site privilege confusion.
- IDOR through guessed or leaked resource IDs.
- Privilege escalation in membership, role, or invitation workflows.
- API key theft, replay, scope bypass, and improper revocation.
- Unauthorized media upload/retrieval or cross-site references.
- Directus Studio exposure or infrastructure credential leakage.
- Sensitive data exposure through logs, errors, exports, or support tooling.

## Mandatory controls
- Server-side authentication and authorization on every protected operation.
- Tenant/site context derived from verified principals or credentials.
- Least-privilege role and API-key scopes.
- Hash-only API key storage and one-time display.
- Input validation and output shaping at trust boundaries.
- Rate limiting and abuse controls for public API and authentication.
- Audit events for privileged actions and credential lifecycle.
- Secrets manager; no secrets in source control.
- Secure media ownership and access checks.
- Dependency, static, integration, and security tests in CI.

## Mandatory negative tests
- Organization A cannot read or mutate Organization B resources.
- Workspace membership cannot grant access beyond its scope.
- Site A API key cannot access Site B content or media.
- Changing resource IDs cannot bypass ownership checks.
- Lower-privileged roles cannot invoke administrative capabilities.
- Revoked/expired keys are rejected.
- Tenant identities cannot access Directus Studio.
- Media references cannot exfiltrate assets across sites.

## RLS caution
PostgreSQL RLS is defense in depth only after the real Directus connection role, bypass privileges, session context propagation, and transaction behavior are verified. Application authorization remains mandatory.
