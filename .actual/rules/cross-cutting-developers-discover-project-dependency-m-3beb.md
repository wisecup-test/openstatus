---
version: 2
name: cross-cutting-developers-discover-project-dependency-m-3beb
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Developers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 3bebc5d3-9033-40b1-9d8e-d7ef88250927
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Developers Discover Project Dependency Manifest Lock

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Developers MUST discover the project dependency manifest and lock resolution artifact to determine and verify the exact resolved version of internal workspace dependencies before implementing or updating notification schema consumers.

### Verify

```bash
# Discover workspace dependency resolution artifact and internal schema references
# Discover and execute project verification script for static type checking
# Discover and execute project test runner for notification dispatch and mock test suites
```

**Accept when:**
- All notification provider implementations parse event payload data through schema validators imported from the shared schema module without type or runtime errors.
- Test mock suites execute successfully and verify payload compliance using identical shared schema definitions.
- Static type analysis and automated test suites across notification packages pass cleanly without unhandled validation rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>