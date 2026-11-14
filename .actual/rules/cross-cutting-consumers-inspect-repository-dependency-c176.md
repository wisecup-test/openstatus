---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-c176
description: "Adoption of @openstatus/db for Database Schema Definitions: Consumers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: c1768612-58f4-4805-a5d7-d1c3ed7d9959
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Consumers Inspect Repository Dependency Manifest Lock

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for modules handling event tracking, telemetry ingestion, persistent record creation, and internal packages interacting directly with database persistence schemas.

### Rules

- **R-OPN-001** MUST: Consumers MUST inspect the repository dependency manifest and lock artifact to verify the exact resolved version and workspace linkage of internal dependencies prior to implementation.

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