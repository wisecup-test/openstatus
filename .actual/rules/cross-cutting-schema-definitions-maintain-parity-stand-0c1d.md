---
version: 2
name: cross-cutting-schema-definitions-maintain-parity-stand-0c1d
description: "Drizzle ORM SQLite Schema Modeling: Schema Definitions Maintain Parity Standard Authentication"
metadata:
  actual:
    adr_id: 0c1d7c81-ddea-4a2b-a7be-28c34f550df6
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Schema Definitions Maintain Parity Standard Authentication

Scope: applies to `packages/db/src/schema/viewers/**`.

These rules are ALWAYS ACTIVE for all data persistence schemas and table definitions utilizing SQLite within the repository data layer, specifically authentication, session, and user identity persistence models.

### Rules

- **R-DRIZZLE-001** SHOULD: Schema definitions maintain parity with standard authentication adapter requirements when modeling user identity and session persistence.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code reviews.
</enforcement>