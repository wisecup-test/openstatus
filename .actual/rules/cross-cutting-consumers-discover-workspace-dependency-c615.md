---
version: 2
name: cross-cutting-consumers-discover-workspace-dependency-c615
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Consumers Discover Workspace Dependency Manifest Repository"
metadata:
  actual:
    adr_id: c615e229-35a5-4116-b407-c3fa14bf8d7d
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Consumers Discover Workspace Dependency Manifest Repository

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered endpoints, route handlers, metadata generation modules, workspace packages, and applications that execute database queries or consume shared entity schema definitions.

### Rules

- **R-DB-001** MUST: Consumers MUST discover the workspace dependency manifest and repository lock artifact to verify resolved dependency versions for database and runtime packages before invoking persistent APIs.
- **R-DB-002** MUST: Import database client instances and schema structures through the primary export entry points provided by @openstatus/db rather than deep-linking into unexported internal files.
- **R-DB-003** MUST: Extract request parameters from request headers and pass them as sanitized inputs into queries constructed through the shared database module when constructing query filters inside server route handlers.

### Verify

```bash
# Discover workspace dependency manifest and execute project type-checking and linting suites
# (Tool/command names must be derived from the repository manifest)
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>