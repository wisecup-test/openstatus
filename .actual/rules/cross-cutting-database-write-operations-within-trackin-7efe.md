---
version: 2
name: cross-cutting-database-write-operations-within-trackin-7efe
description: "Adoption of @openstatus/db for Database Schema Definitions: Database Write Operations Within Tracking Routines"
metadata:
  actual:
    adr_id: 7efe2696-de50-4400-979c-6a67df5fca22
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Database Write Operations Within Tracking Routines

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all modules handling event tracking, telemetry ingestion, and persistent record creation, as well as internal packages interacting directly with database persistence schemas.

### Rules

- **R-DB-001** SHOULD: All database write operations within tracking routines validate event payloads against the exported @openstatus/db schema contracts at the ingestion boundary.

### Verify

```bash
# Discover the workspace package runner and execute type verification across tracking modules
# Discover and run automated unit and integration test suites for tracking services
```

**Accept when:**
- Type verification confirms that tracking routines adhere to all imported database schema contracts without compiler errors.
- All automated test suites for tracking and telemetry operations complete successfully.
- No redundant database schema definitions are present within downstream tracking modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>