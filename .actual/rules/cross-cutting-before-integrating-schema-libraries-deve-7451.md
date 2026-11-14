---
version: 2
name: cross-cutting-before-integrating-schema-libraries-deve-7451
description: "Drizzle ORM SQLite Schema Modeling: Before Integrating Schema Libraries Developers Inspect"
metadata:
  actual:
    adr_id: 7451afce-552d-4265-9d2c-6b62f9a327a4
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Before Integrating Schema Libraries Developers Inspect

Scope: applies to `packages/db/src/schema/viewers/**`.

These rules are ALWAYS ACTIVE for all data persistence schemas and table definitions utilizing SQLite within the repository data layer, and authentication, session, and user identity persistence models.

### Rules

- **R-DRZ-001** MUST: Before integrating schema libraries, developers MUST inspect the repository dependency resolution lock artifact to identify the exact resolved version and verify supported API signatures.

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