# Architecture

## System boundary
The product owns customer experience, API contracts, identity context, authorization, validation, business rules, audit policy, and tenant resolution. Directus provides content/data infrastructure behind a controlled adapter.

## Logical flow
```text
SuperAdmin / Customer Apps / External Consumers
                     |
               Product API / BFF
                     |
             Application Services
       (Identity, Tenants, Sites, CMS,
          API Keys, Media, Audit)
                     |
               Directus Adapter
                     |
             Directus + PostgreSQL
```

Directus Studio is a separate private operational surface, not a customer admin panel.

## Architectural style
Begin with a modular monolith. Modules communicate through explicit application interfaces. Avoid premature microservices and prevent Directus-specific types from leaking into product contracts.

## Domain and decisions
See [Domain Model](domain-model.md), [Product Surfaces](surfaces.md), and [ADR index](../adr/README.md). Technology selections not yet approved remain open until Spec Kit clarification and planning.
