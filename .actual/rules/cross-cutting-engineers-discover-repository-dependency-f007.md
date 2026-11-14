---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-f007
description: "Adoption of @openstatus/db for Shared Persistence and Schema Management: Engineers Discover Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: f00716dd-d91c-4103-bc69-40b70bf54236
    status: accepted
    candidate_group_id: 4e974e32-9858-4cf2-979b-8d161892d22e
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/db for Shared Persistence and Schema Management: Engineers Discover Repository Dependency Resolution Artifact

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API route handlers, router definitions, and domain controllers requiring database interactions or relational schema references.

### Rules

- **R-DB-001** MUST: Engineers MUST discover the repository dependency resolution artifact to determine the exact locked version of all workspace packages and external dependencies prior to implementation.

### Verify

```bash
# Discover and execute the project automated test suite to validate database interactions and router handlers.
# Discover and run the project type checking and static analysis scripts to ensure compliant schema imports across all routers.
# Discover and execute the project linting scripts to verify that direct database client instantiations are absent from router modules.
```

**Accept when:**
- The discovered automated test suite passes with all database-dependent route handlers executing queries through context.
- Static analysis and type checking scripts complete with zero errors related to schema imports from @openstatus/db.
- Static inspection confirms no unauthorized database client instantiations exist outside the centralized persistence package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and peer code review enforce that all database queries use context-injected clients and schemas from @openstatus/db.
</enforcement>