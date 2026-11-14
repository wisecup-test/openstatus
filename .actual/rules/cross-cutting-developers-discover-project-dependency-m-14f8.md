---
version: 2
name: cross-cutting-developers-discover-project-dependency-m-14f8
description: "Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Developers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 14f827a0-3e55-4499-b73b-04df4b489554
    status: accepted
    candidate_group_id: 01f978a3-302d-4828-9836-80d54041a4a9
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
      - src/workspace/**
---

# Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Developers Discover Project Dependency Manifest Lock

Scope: applies to `packages/services/src/import/**`, `packages/services/src/workspace/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project dependency manifest and lock artifact to inspect and verify the exact resolved version of internal and external dependencies before implementing or modifying schema consumers.

### Verify

```bash
# Discover and execute the repository static type checker and linting suite
# Discover and execute the workspace test runner
```

**Accept when:**
- All service modules consuming database entities and plan definitions import them directly from the centralized database schema module.
- The project static analysis and test validation suites pass with zero errors across all workspace packages that consume the shared database schema.

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace packages consuming shared database schemas must be verified via the repository static type checker, linting suite, and test runner.
</enforcement>