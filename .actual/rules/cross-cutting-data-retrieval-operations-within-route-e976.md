---
version: 2
name: cross-cutting-data-retrieval-operations-within-route-e976
description: "Adoption of @openstatus/db for Shared Persistence and Schema Management: Data Retrieval Operations Within Route Handlers"
metadata:
  actual:
    adr_id: e976a373-e3f0-4b5a-ba0e-2f68dc410b23
    status: accepted
    candidate_group_id: 4e974e32-9858-4cf2-979b-8d161892d22e
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/db for Shared Persistence and Schema Management: Data Retrieval Operations Within Route Handlers

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API route handlers, router definitions, and domain controllers requiring database interactions or relational schema references.

### Rules

- **R-DB-001** SHOULD: Data retrieval operations within route handlers use the structured query builder interface exposed by the context database client rather than raw query strings.

### Verify

```bash
# Discover and run the project automated test suite
# Discover and run type checking and static analysis scripts
# Discover and run linting scripts to verify absence of direct database client instantiations
```

**Accept when:**
- The discovered automated test suite passes with all database-dependent route handlers executing queries through context.
- Static analysis and type checking scripts complete with zero errors related to schema imports from @openstatus/db.
- Static inspection confirms no unauthorized database client instantiations exist outside the centralized persistence package.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database queries must use context-injected clients and schemas from @openstatus/db.
</enforcement>