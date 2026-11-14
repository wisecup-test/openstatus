---
version: 2
name: cross-cutting-notification-provider-configuration-defi-fc1d
description: "Zod Schema Validation for Notification Configurations: Notification Provider Configuration Definitions Use Zod"
metadata:
  actual:
    adr_id: fc1d0bdb-fdf1-42c3-8754-7c991095fb90
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Notification Provider Configuration Definitions Use Zod

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Notification provider configuration definitions MUST use Zod declarative object schemas to validate all configuration parameters before initializing provider clients.

### Verify

```bash
# Discover and execute the repository type-checking script across integration modules to verify schema typings.
# Discover and run the project test suite covering notification configuration parsing and validation.
```

**Accept when:**
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>