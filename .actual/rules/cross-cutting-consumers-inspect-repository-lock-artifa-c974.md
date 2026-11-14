---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-c974
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: c974265e-d3c7-4517-88f9-237892fa1654
    status: accepted
    candidate_group_id: 8a25c2df-024a-4181-8d98-527a4ff68ed7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side application route handlers, background execution entry points, and shared procedure callers requiring database access or relational schema validation.

### Rules

- **R-DB-001** MUST: Consumers MUST inspect the repository lock artifact and verify the exact resolved dependency definitions prior to consuming the shared package.

### Verify

```bash
# Discover and run repository static analysis and type verification scripts
# Discover and run the project automated test suite
```

**Accept when:**
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Direct database driver instantiations or unapproved local schema definitions are blocked from merging until corrected.
</enforcement>