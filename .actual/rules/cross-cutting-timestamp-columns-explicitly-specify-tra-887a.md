---
version: 2
name: cross-cutting-timestamp-columns-explicitly-specify-tra-887a
description: "Drizzle ORM SQLite Schema Modeling: Timestamp Columns Explicitly Specify Transformation Modes"
metadata:
  actual:
    adr_id: 887a98aa-4516-4a30-9afa-4e38215e129b
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Timestamp Columns Explicitly Specify Transformation Modes

Scope: applies to `packages/db/src/schema/viewers/**`.

These rules are ALWAYS ACTIVE for all data persistence schemas and table definitions utilizing SQLite within the repository data layer, including authentication, session, and user identity persistence models.

### Rules

- **R-DRIZZLE-001** MUST: Timestamp columns MUST explicitly specify timestamp transformation modes or SQL default expressions to avoid ambiguous datetime representations in storage.

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
Claude Code MUST NOT skip or defer verification. All changes must pass static type checking, schema validation, and automated tests confirming proper timestamp handling and referential integrity.
</enforcement>