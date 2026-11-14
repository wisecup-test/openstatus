---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-fafa
description: "Adoption of @openstatus/notification-base for Notification Handlers: Consumers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: fafa2c21-42a3-4b08-ba94-abb13a12aeff
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Consumers Inspect Repository Dependency Manifest Resolution

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for notification handler packages, dispatch services, and workspace modules implementing provider integrations governed by internal notification contracts.

### Rules

- **R-NOTIF-001** MUST: Consumers MUST inspect the repository dependency manifest and resolution artifact to resolve the exact locked version of @openstatus/notification-base prior to implementation.

### Verify

```bash
# Discover and execute the project's static analysis and type verification script from the repository manifest to verify @openstatus/notification-base contract conformance.
# Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior.
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, mandatory architectural and code reviews, and blocking of pull requests bypassing @openstatus/notification-base or hardcoding credentials enforce these rules.
</enforcement>