---
version: 2
name: cross-cutting-database-persistence-operations-within-r-5a5f
description: "Adoption of @openstatus/db for Shared Persistence and Schema Management: Database Persistence Operations Within Routers Consume"
metadata:
  actual:
    adr_id: 5a5f8d42-0a94-4981-bd8c-05accf69eabe
    status: accepted
    candidate_group_id: 4e974e32-9858-4cf2-979b-8d161892d22e
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/db for Shared Persistence and Schema Management: Database Persistence Operations Within Routers Consume

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API route handlers, router definitions, and domain controllers requiring database interactions or relational schema references.

### Rules

- **R-DB-001** MUST: All database persistence operations within API routers MUST consume the database client provided by the request context and MUST import entity schemas from @openstatus/db.

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