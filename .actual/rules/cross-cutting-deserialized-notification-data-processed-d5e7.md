---
version: 2
name: cross-cutting-deserialized-notification-data-processed-d5e7
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: Deserialized Notification Data Processed Through Zod"
metadata:
  actual:
    adr_id: d5e73540-0bd5-4161-9d21-49ab835a0185
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: Deserialized Notification Data Processed Through Zod

Scope: applies to `packages/notifications/webhook/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Deserialized notification data MUST be processed through the Zod schema parse method before domain logic accesses any payload attributes.

### Verify

```bash
# Discover and execute static analysis, type checking, and the automated test suite
# (Refer to repository package manifests and build configuration for exact tool invocation commands)
```

**Accept when:**
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>