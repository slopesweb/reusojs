# reusojs

A Directus-first, multi-tenant headless CMS foundation. The first proof of concept validates organization, workspace, site, content, and site-scoped REST API boundaries.

> Status: architecture and specification bootstrap. Runtime implementation has not started.

## Project documentation

- [Project context](PROJECT_CONTEXT.md)
- [First feature specification](specs/001-multitenant-cms-foundation/spec.md)
- [Clarification register](specs/001-multitenant-cms-foundation/clarifications.md)
- [Architecture](docs/architecture/README.md)
- [Domain model](docs/architecture/domain-model.md)
- [Architecture decisions](docs/adr/README.md)
- [Security model](docs/security/README.md)
- [Development workflow](CONTRIBUTING.md)

## Spec-Driven Development

This repository adopts [GitHub Spec Kit](https://github.com/github/spec-kit). Install the Specify CLI and initialize the existing repository for Codex with:

```bash
uv tool install specify-cli
specify init --here --force --integration codex
```

This repository already has a committed baseline. `--force` is required because the directory is non-empty and may replace managed paths; inspect the complete diff and preserve the agreed constitution before committing. The project constitution is maintained at `.specify/memory/constitution.md`.

## Language policy

Repository documentation, specifications, code comments, identifiers, and technical artifacts are written in English. Product discussions may be conducted in Brazilian Portuguese.
