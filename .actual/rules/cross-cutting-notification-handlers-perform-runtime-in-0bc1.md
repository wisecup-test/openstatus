---
version: 2
name: cross-cutting-notification-handlers-perform-runtime-in-0bc1
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Handlers Perform Runtime Input Parsing"
metadata:
  actual:
    adr_id: 0bc13f87-bff4-4173-9c0d-b2da0ef3c3f2
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Handlers Perform Runtime Input Parsing

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification handlers MUST perform runtime input parsing and validation on incoming serialized payload strings using the designated schema parser before dispatching external client requests.

### Verify

```bash
# Discover the workspace dependency resolution artifact and verify all internal schema package references resolve consistently.
# Discover and execute the project verification script for static type checking across notification packages.
# Discover and execute the project test runner to validate notification dispatch and mock test suites against shared schema contracts.
```

**Accept when:**
- All notification provider implementations parse event payload data through schema validators imported from the shared schema module without type or runtime errors.
- Test mock suites execute successfully and verify payload compliance using identical shared schema definitions.
- Static type analysis and automated test suites across notification packages pass cleanly without unhandled validation rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider implementations must consume shared schema contracts and validate inbound payloads at runtime.
</enforcement>