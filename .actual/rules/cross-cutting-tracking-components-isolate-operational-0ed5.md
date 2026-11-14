---
version: 2
name: cross-cutting-tracking-components-isolate-operational-0ed5
description: "Adoption of @openstatus/db for Database Schema Definitions: Tracking Components Isolate Operational Parameters Filtering"
metadata:
  actual:
    adr_id: 0ed5ff3b-3447-4702-9134-933f2a022fb6
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Tracking Components Isolate Operational Parameters Filtering

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Tracking components MUST isolate operational parameters and filtering rules into dedicated configuration modules distinct from persistent database entity schemas.

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