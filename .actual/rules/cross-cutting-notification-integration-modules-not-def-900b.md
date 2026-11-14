---
version: 2
name: cross-cutting-notification-integration-modules-not-def-900b
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Integration Modules Not Define Standalone"
metadata:
  actual:
    adr_id: 900bdcc2-d924-48b7-973d-655939aaa215
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Integration Modules Not Define Standalone

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification provider integration modules handling inbound event payloads, and test mocks and verification suites simulating notification data dispatch.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification integration modules MUST NOT define standalone or ad-hoc duplicate schema types for domain entities managed by the central schema module.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines execute workspace-wide type checks and test suites, and pull requests introducing unvalidated notification payload ingestion or duplicated schema definitions will fail automated checks and be blocked from merge.
</enforcement>