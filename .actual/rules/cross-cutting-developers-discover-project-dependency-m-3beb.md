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

These rules are ALWAYS ACTIVE for notification provider integration modules handling inbound event payloads and test mocks/verification suites simulating notification data dispatch.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project dependency manifest and lock resolution artifact to determine and verify the exact resolved version of internal workspace dependencies before implementing or updating notification schema consumers.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code reviews enforce workspace-wide type checks, test suites, and shared schema consumption.
</enforcement>