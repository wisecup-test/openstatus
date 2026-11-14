---
version: 2
name: cross-cutting-integration-test-harnesses-mock-implemen-46ce
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Integration Test Harnesses Mock Implementations Consume"
metadata:
  actual:
    adr_id: 46ce4f42-900e-4af7-b3dc-af74494d3faf
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Integration Test Harnesses Mock Implementations Consume

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all notification provider integration modules, test mocks, and verification suites handling inbound event payloads.

### Rules

- **R-NOTIF-001** SHOULD: Integration test harnesses and mock implementations SHOULD consume the identical schema definitions and validation routines from the shared database schema module to guarantee test parity with production handlers.

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
Claude Code MUST NOT skip or defer verification. Verification by automated CI pipelines and peer code reviews is mandatory. Pull requests introducing unvalidated notification payload ingestion or duplicated schema definitions will fail automated checks and be blocked from merge.
</enforcement>