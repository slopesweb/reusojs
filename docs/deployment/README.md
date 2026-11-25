# Deployment Notes

Deployment topology is undecided. Spec Kit planning must evaluate local development, CI, staging, and production.

Minimum boundaries:
- Directus Studio is not publicly available to tenant users.
- Product API and customer applications are independently addressable from Directus Studio.
- PostgreSQL credentials follow least privilege.
- Secrets are injected through a deployment secret manager.
- Backups, restore drills, migrations, observability, and incident response are documented before production.
- Staging exercises the same authorization and database-role behavior as production.
