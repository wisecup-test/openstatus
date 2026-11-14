---
version: 2
name: cross-cutting-before-implementing-modifying-code-schem-e1d4
description: "Zod Schema Validation for Subscription Channel Configurations and Payloads: Before Implementing Modifying Code Schema Validation"
metadata:
  actual:
    adr_id: e1d4da9a-ae9d-4cee-9c40-63c634423a82
    status: accepted
    candidate_group_id: 500f8313-2cab-4db3-a165-4a1aab0ad2dd
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Zod Schema Validation for Subscription Channel Configurations and Payloads: Before Implementing Modifying Code Schema Validation

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all subscription notification channel adapters handling deserialized configuration data and all status report and event notification payload definitions dispatched to external targets.

### Rules

- **R-ZOD-001** MUST: Before implementing or modifying code using the schema validation library, consumers MUST inspect the repository lock artifact to determine the exact resolved library version and verify API availability against that version.

### Verify

```bash
test -n "${VERIFY_SCHEMA_VALIDATION:-}" && eval "${VERIFY_SCHEMA_VALIDATION}" || echo "Discover and execute project test suite for schema validation compliance"
test -n "${VERIFY_CODE_STANDARDS:-}" && eval "${VERIFY_CODE_STANDARDS}" || echo "Discover and execute project static analysis and linting checks"
```

**Accept when:**
- All channel configuration parsing and payload formatting paths pass automated boundary validation tests with valid schemas.
- Invalid configuration strings or corrupted event payloads fail safely without uncaught runtime exceptions during dispatch.
- The locked dependency version for the schema validation library is verified against repository lock artifacts prior to execution.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>