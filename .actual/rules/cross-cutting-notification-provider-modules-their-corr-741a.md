---
version: 2
name: cross-cutting-notification-provider-modules-their-corr-741a
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Provider Modules Their Corresponding Test"
metadata:
  actual:
    adr_id: 741a22b2-3034-445f-b770-035a98c1ec42
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Provider Modules Their Corresponding Test

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Notification provider modules and their corresponding test harnesses MUST import and validate domain payload structures using schema definitions provided by @openstatus/db/src/schema.

### Verify

```bash
# Discover workspace dependency resolution artifact and verify schema references
# Discover and execute project static type checking across notification packages
# Discover and execute test runner to validate notification dispatch and mock test suites
```

**Accept when:**
- All notification provider implementations parse event payload data through schema validators imported from the shared schema module without type or runtime errors.
- Test mock suites execute successfully and verify payload compliance using identical shared schema definitions.
- Static type analysis and automated test suites across notification packages pass cleanly without unhandled validation rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code reviews enforce these rules, and violations will block pull request merges.
</enforcement>