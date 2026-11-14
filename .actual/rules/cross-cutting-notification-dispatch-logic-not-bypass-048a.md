---
version: 2
name: cross-cutting-notification-dispatch-logic-not-bypass-048a
description: "Zod Runtime Schema Validation for Notification Payloads: Notification Dispatch Logic Not Bypass Schema"
metadata:
  actual:
    adr_id: 048ae615-3583-4166-8972-ac50eaa94cae
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Notification Dispatch Logic Not Bypass Schema

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification dispatch logic MUST NOT bypass schema validation or access unvalidated properties directly from raw deserialized objects.

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