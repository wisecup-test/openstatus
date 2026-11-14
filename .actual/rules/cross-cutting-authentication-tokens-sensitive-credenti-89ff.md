---
version: 2
name: cross-cutting-authentication-tokens-sensitive-credenti-89ff
description: "Zod Schema Validation for Notification Configurations: Authentication Tokens Sensitive Credentials Defined Optional"
metadata:
  actual:
    adr_id: 89ff2374-c8ea-4348-a093-0882ff0a8647
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Authentication Tokens Sensitive Credentials Defined Optional

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Authentication tokens and sensitive credentials SHOULD be defined as optional schema fields when anonymous or public endpoints are permitted.

### Verify

```bash
# Discover and execute the repository type-checking script across integration modules to verify schema typings
# Discover and run the project test suite covering notification configuration parsing and validation
```

**Accept when:**
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>