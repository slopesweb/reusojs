# ADR-0001: Directus-First Product Boundary

- Status: Accepted
- Date: 2026-11-25

## Context
The product needs a reusable content/data engine without turning its infrastructure console into the customer SaaS experience.

## Decision
Use Directus as an internal infrastructure engine behind an application-owned adapter. Keep Directus Studio private as the GodMode operations surface. Customer SuperAdmin, Customer Admin, Content User, and external consumers use product-owned applications and API contracts.

## Consequences
- Product authentication, authorization, tenant resolution, validation, business rules, and response contracts remain application-owned.
- Directus-specific details do not leak into public API guarantees.
- Directus Studio access is restricted to authorized internal operators.
- Customer UI is independently designed and implemented.
