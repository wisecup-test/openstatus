---
version: 2
name: cross-cutting-notification-payload-schemas-compose-sha-3ccd
description: "Zod Schema Validation for Subscription Channel Configurations and Payloads: Notification Payload Schemas Compose Shared Domain"
metadata:
  actual:
    adr_id: 3ccd423c-ae9c-43fa-aa2c-d34858822857
    status: accepted
    candidate_group_id: 500f8313-2cab-4db3-a165-4a1aab0ad2dd
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Zod Schema Validation for Subscription Channel Configurations and Payloads: Notification Payload Schemas Compose Shared Domain

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all subscription notification channel adapters handling deserialized configuration data and all status report and event notification payload definitions dispatched to external targets.

### Rules

- **R-ZOD-001** SHOULD: Notification payload schemas SHOULD compose shared domain validation schemas to prevent divergent schema definitions across distinct channel implementations.
- **R-ZOD-002** MANDATORY: Define channel configuration schemas alongside their respective channel handlers to maintain high cohesion.
- **R-ZOD-003** MANDATORY: Use non-throwing safe parsing methods when evaluating configuration data to handle malformed input gracefully.
- **R-ZOD-004** MANDATORY: Compose complex event payload schemas from reusable domain primitive schemas rather than defining inline duplicates.
- **R-ZOD-005** MANDATORY: Prior to writing code that uses a versioned library, inspect repository lock or resolution artifacts to determine the exact resolved version, and confirm every API, class, or function exists in that exact version's documentation.

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
Claude Code MUST NOT skip or defer verification. Merge requests that deserialize external channel configurations without schema validation MUST be blocked. Runtime validation failures must be logged at the error level and fail safely without taking down dispatch workers.
</enforcement>