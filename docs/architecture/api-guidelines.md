# REST API Guidelines

## Contract ownership
The product owns the public REST API. Directus endpoints are not a public compatibility contract.

## Versioning
Initial base path: `/v1`.

## Initial endpoints
- `GET /v1/posts`
- `GET /v1/posts/{id}`
- `POST /v1/posts`
- `PATCH /v1/posts/{id}`
- `DELETE /v1/posts/{id}`
- `GET /v1/categories`
- `POST /v1/categories`
- `GET /v1/tags`
- `GET /v1/media`

Payloads, filtering, pagination, sorting, error envelope, and public-vs-management authorization must be specified and captured in OpenAPI.

## Context and authorization
- Human sessions and machine API keys are distinct principal types.
- API keys are scoped to one Site; site context is derived from credential record.
- Request `site_id` or `organization_id` is never proof of access.
- Public reads of published content and private management operations use separate policies.
- Every lookup constrains ownership before returning or mutating a record.

## API key lifecycle
Generate cryptographically secure random secrets. Store only a strong one-way hash and non-secret lookup identifier/prefix. Display the secret once at creation. Support individual revocation, scopes, optional expiry, and attributable usage/audit records. Never log raw secrets.
