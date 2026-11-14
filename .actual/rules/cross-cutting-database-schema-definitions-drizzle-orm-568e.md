---
version: 2
name: cross-cutting-database-schema-definitions-drizzle-orm-568e
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly"
metadata:
  actual:
    adr_id: 568e31f9-58cf-4d85-9f0d-b3058eff0e72
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier, including relational entities representing multi-tenant resources, event telemetry, external service components, and status reports (excluding in-memory ephemeral caches, stateless client-side data structures, transit payload models, and single-row configuration tables or static lookup enumerations where table scans incur negligible operational cost under EXC-001).

### Rules

- **R-DRIZZLE-001** MUST: Database schema definitions using Drizzle ORM MUST explicitly declare index and uniqueness constraints within table configuration callbacks for all multi-tenant partition keys, external entity integrations, and timestamped query boundaries.
- **R-DRIZZLE-002** DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-DRIZZLE-003** LOCK-VERSION GROUNDING (MANDATORY): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official docs/changelog for that exact version. (5) Confirm every API, class, or function exists in that exact version. (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-DRIZZLE-004** MUST: Declare table indices within the second argument configuration callback of schema builders using typed table column references.
- **R-DRIZZLE-005** MUST: Place the tenant partition column as the leading element in composite index definitions to maximize index selectivity for tenant-scoped queries.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration automated schema verification, static type checking, and mandatory peer review on all schema definitions and migration diffs. Violations block pull requests and deployment.
</enforcement>