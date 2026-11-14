---
version: 2
name: cross-cutting-tracking-telemetry-modules-consume-datab-6a79
description: "Adoption of @openstatus/db for Database Schema Definitions: Tracking Telemetry Modules Consume Database Entity"
metadata:
  actual:
    adr_id: 6a79ac3f-0021-458d-8fbd-6545d1e34715
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Tracking Telemetry Modules Consume Database Entity

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Tracking and telemetry modules MUST consume database entity definitions directly from @openstatus/db schema exports rather than defining standalone or duplicate database models.

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