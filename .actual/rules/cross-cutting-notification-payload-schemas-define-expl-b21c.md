---
version: 2
name: cross-cutting-notification-payload-schemas-define-expl-b21c
description: "Zod Runtime Schema Validation for Notification Payloads: Notification Payload Schemas Define Explicit Field"
metadata:
  actual:
    adr_id: b21caefc-8074-4b5a-82e4-4e64f444ebdf
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Notification Payload Schemas Define Explicit Field

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Notification payload schemas MUST define explicit field validations using zod object schemas, including appropriate field types, defaults, and optionality.

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