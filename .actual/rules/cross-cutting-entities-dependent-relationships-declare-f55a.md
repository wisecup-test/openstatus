---
version: 2
name: cross-cutting-entities-dependent-relationships-declare-f55a
description: "Drizzle ORM SQLite Schema Modeling: Entities Dependent Relationships Declare Foreign Key"
metadata:
  actual:
    adr_id: f55ac0f0-3ebf-4e37-9d22-3fb43093173f
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Entities Dependent Relationships Declare Foreign Key

Scope: applies to `packages/db/src/schema/viewers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Entities with dependent relationships MUST declare foreign key constraints with explicit cascade rules to guarantee referential integrity at the database engine level.
- **R-DRIZZLE-002** MANDATORY: Execute the discovery policy to find dependency manifests, build tools, lock files, and version-specific documentation before writing code that uses a versioned library.

### Verify

```bash
# Discover and run the project type checking script to verify schema type correctness across all consuming packages.
# Discover and run the database schema verification or migration generation command to validate table constraints.
# Discover and run the automated test suite to confirm session cascading and timestamp deserialization.
```

**Accept when:**
- Static type analysis passes with zero errors across all database schema definitions and downstream consumers.
- Database schema migration checks confirm valid SQLite DDL generation with foreign key references intact.
- Integration tests succeed confirming cascade deletion of session records upon user deletion.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>