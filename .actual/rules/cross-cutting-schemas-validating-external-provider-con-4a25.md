---
version: 2
name: cross-cutting-schemas-validating-external-provider-con-4a25
description: "Zod Runtime Schema Validation for Notification Payloads: Schemas Validating External Provider Configurations Declare"
metadata:
  actual:
    adr_id: 4a253ab6-7883-4605-8076-b681cd87b738
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Schemas Validating External Provider Configurations Declare

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Schemas validating external provider configurations SHOULD declare fallback defaults via schema-level default or prefault configurations for optional connection parameters.

### Verify

```bash
# Discover the project test runner script from the repository manifest and execute the test suite validating payload schema enforcement.
# Discover the repository linter script from the root configuration and execute static analysis across notification integration modules.
```

**Accept when:**
- All notification payload validation tests pass successfully with valid and invalid input fixtures.
- Static analysis and type checking pass without violations across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>