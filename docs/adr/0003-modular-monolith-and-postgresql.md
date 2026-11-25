# ADR-0003: Modular Monolith and PostgreSQL

- Status: Accepted
- Date: 2026-11-25

## Context
The proof of concept must validate multi-tenancy and reusable SaaS foundations without distributed-system overhead.

## Decision
Start with a modular monolith and PostgreSQL as mandatory system of record. Use a shared database deployment with logical tenant isolation. Supabase is optional infrastructure, not a required dependency. Do not provision a database or schema per customer for the initial MVP.

## Consequences
- Application modules have explicit interfaces and ownership.
- Cross-tenant security tests are mandatory.
- RLS may be defense in depth only after actual Directus DB role and connection path are verified.
- Microservices or per-tenant databases require a future ADR supported by product and operational evidence.
