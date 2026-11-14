---
version: 2
name: cross-cutting-provider-configuration-schemas-define-de-0eb4
description: "Zod Schema Validation for Notification Configurations: Provider Configuration Schemas Define Destination Topics"
metadata:
  actual:
    adr_id: 0eb4d62a-7376-485c-bdb9-41fce3e048d1
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Provider Configuration Schemas Define Destination Topics

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Provider configuration schemas MUST define destination topics and server connection URLs as string schemas with standard default fallbacks where provider conventions exist.

### Verify

```bash
# Discover and execute the repository type-checking script across integration modules
# Discover and run the project test suite covering notification configuration parsing and validation
```

**Accept when:**
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>