---
version: 2
name: cross-cutting-notification-integrations-parsing-extern-dba0
description: "Zod Runtime Schema Validation for Notification Payloads: Notification Integrations Parsing External Persisted Payload"
metadata:
  actual:
    adr_id: dba0baab-e84e-4a0c-82bd-c64e0d42f564
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Notification Integrations Parsing External Persisted Payload

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All notification integrations parsing external or persisted payload data MUST validate the parsed payload structure using zod schemas before processing or dispatching requests.

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