---
version: 2
name: cross-cutting-complex-entity-graphs-requiring-related-c773
description: "Centralized Relational Data Access with @openstatus/db: Complex Entity Graphs Requiring Related Records"
metadata:
  actual:
    adr_id: c773a457-28e4-46ed-88a9-1c8777ced26e
    status: accepted
    candidate_group_id: 0e1a37eb-0f9b-4ab0-9e8c-680ac9ec4729
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/auth/**
      - src/router/**
---

# Centralized Relational Data Access with @openstatus/db: Complex Entity Graphs Requiring Related Records

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, authentication helpers, and data access logic requiring multi-tenant scoping or relational queries.

### Rules

- **R-DB-001** SHOULD: Complex entity graphs requiring related records SHOULD be retrieved using declarative relational queries with explicit relation selectors rather than manual procedural joins.
- **R-DB-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DB-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-DB-004** MANDATORY: Construct relational queries with explicit column and relation constraints to minimize payload sizes and prevent unindexed joins.
- **R-DB-005** MANDATORY: Ensure all mutations and conditional queries on tenant-owned entities include workspace identifiers extracted from authenticated procedure contexts.

### Verify

```bash
# Discover and run the project static type analysis verification script from the repository manifest
pnpm typecheck
# Execute the repository test validation command discovered from the project configuration
pnpm test
# Run the project structural linting check discovered from the build configuration
pnpm lint
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must pass automated continuous integration pipelines executing static type analysis, test suites, and lint checks enforcing centralized database usage and tenant isolation.
</enforcement>