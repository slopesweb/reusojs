# reusojs

A Directus-first, multi-tenant headless CMS foundation. The first proof of concept validates organization, workspace, site, content, and site-scoped REST API boundaries.

> Status: architecture and specification bootstrap. Runtime implementation has not started.

## Project documentation

- [Project context](PROJECT_CONTEXT.md)
- [Architecture](docs/architecture/README.md)
- [Domain model](docs/architecture/domain-model.md)
- [Architecture decisions](docs/adr/README.md)
- [Security model](docs/security/README.md)
- [Development workflow](CONTRIBUTING.md)

## Spec-Driven Development

This repository adopts [GitHub Spec Kit](https://github.com/github/spec-kit). Install the Specify CLI and initialize the existing repository for Codex with:

```bash
uv tool install specify-cli
specify init --here --integration codex
```

Review generated files before committing. The project constitution is maintained at `.specify/memory/constitution.md`.

## Language policy

Repository documentation, specifications, code comments, identifiers, and technical artifacts are written in English. Product discussions may be conducted in Brazilian Portuguese.
