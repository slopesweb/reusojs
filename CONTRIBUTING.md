# Contributing

## Setup
Install the Specify CLI and initialize this existing repository for Codex:

```bash
uv tool install specify-cli
specify init --here --integration codex
```

Review generated files before committing. Avoid `--force` unless a reviewable baseline exists; it may replace managed paths.

## Standard Spec Kit workflow
Run each stage separately in the coding agent and review its output:
1. `$speckit-constitution` — establish and maintain principles.
2. `$speckit-specify` — define outcome and requirements.
3. `$speckit-clarify` — resolve material ambiguities.
4. `$speckit-plan` — define architecture and technology.
5. `$speckit-checklist` — assess requirements quality.
6. `$speckit-tasks` — create dependency-ordered work.
7. `$speckit-analyze` — resolve inconsistencies.
8. `$speckit-implement` — implement approved tasks.
9. `$speckit-converge` — verify implementation against artifacts.

Codex invokes installed skills with the `$` prefix. These are agent-chat commands, not terminal commands.

## Quality
Add tests for allowed and denied behavior. Tenant-boundary tests are required for all tenant data access. Run relevant formatting, lint, static analysis, unit, integration, and security checks. Record exact commands, environment, results, and gaps in `VALIDATION.md` or feature validation artifacts.

All committed technical documentation, specs, comments, and identifiers are in English. Update ADRs when accepted architecture changes.
