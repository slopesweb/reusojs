# ADR-0002: Multi-Tenant Domain Hierarchy

- Status: Accepted
- Date: 2026-11-25

## Context
One Organization may operate multiple independent Sites and group them into Workspaces.

## Decision
Adopt `Platform → Organization → Workspace → Site → Content`. Organization is the primary tenant boundary. Every Organization has a default Workspace and may have more. A Workspace belongs to one Organization and may contain multiple Sites. Each Site belongs to exactly one Workspace. Content, categories, tags, media, settings, and API keys are directly Site-scoped.

## Consequences
- Users may join multiple Organizations.
- Membership and Workspace Membership determine contextual access.
- Workspace is an organizational grouping, not a separate tenant boundary.
- Site ownership is unique and inherited through its Workspace.
- Access checks validate Organization, Workspace, Site, and resource ownership as applicable.
