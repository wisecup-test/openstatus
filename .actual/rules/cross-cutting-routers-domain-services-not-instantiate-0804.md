---
version: 2
name: cross-cutting-routers-domain-services-not-instantiate-0804
description: "Adoption of @openstatus/db for Shared Persistence and Schema Management: Routers Domain Services Not Instantiate Independent"
metadata:
  actual:
    adr_id: 08043e0b-32a3-49e6-88b2-3f9204d1f896
    status: accepted
    candidate_group_id: 4e974e32-9858-4cf2-979b-8d161892d22e
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/db for Shared Persistence and Schema Management: Routers Domain Services Not Instantiate Independent

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API route handlers, router definitions, and domain controllers requiring database interactions or relational schema references.

### Rules

- **R-DB-001** MUST_NOT: Routers and domain services MUST NOT instantiate independent database connection clients or establish unmanaged connection pools outside the shared @openstatus/db module.

### Verify

```bash
# Discover and execute the project automated test suite
# Discover and run type checking and static analysis scripts
# Discover and execute project linting scripts to verify absence of direct database client instantiations
```

**Accept when:**
- The discovered automated test suite passes with all database-dependent route handlers executing queries through context.
- Static analysis and type checking scripts complete with zero errors related to schema imports from @openstatus/db.
- Static inspection confirms no unauthorized database client instantiations exist outside the centralized persistence package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code review, and automated checks enforce these rules on every code change.
</enforcement>