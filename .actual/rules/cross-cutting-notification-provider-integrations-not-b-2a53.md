---
version: 2
name: cross-cutting-notification-provider-integrations-not-b-2a53
description: "Zod Schema Validation for Notification Configurations: Notification Provider Integrations Not Bypass Schema"
metadata:
  actual:
    adr_id: 2a53b0ed-8753-44e7-ad01-314d0813c5b1
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Notification Provider Integrations Not Bypass Schema

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Notification provider integrations MUST_NOT bypass schema validation by casting raw unparsed inputs to target configuration types.

### Verify

```bash
# Discover and execute the repository type-checking script across integration modules
npm run typecheck
# Run the project test suite covering notification configuration parsing and validation
npm test
```

**Accept when:**
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider configuration modules accepting external configuration or environment inputs must validate using Zod schemas.
</enforcement>