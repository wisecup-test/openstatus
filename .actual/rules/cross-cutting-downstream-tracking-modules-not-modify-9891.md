---
version: 2
name: cross-cutting-downstream-tracking-modules-not-modify-9891
description: "Adoption of @openstatus/db for Database Schema Definitions: Downstream Tracking Modules Not Modify Extend"
metadata:
  actual:
    adr_id: 98911d7e-0450-415c-aa51-2df155ed2a6c
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Downstream Tracking Modules Not Modify Extend

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Downstream tracking modules MUST NOT modify or extend canonical schema definitions locally; all database schema changes MUST originate within the authoritative @openstatus/db package.

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