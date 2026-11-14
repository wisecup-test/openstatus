---
version: 2
name: cross-cutting-schemas-defining-alert-metadata-declare-d4b1
description: "Zod Schema Definition for Notification Payloads: Schemas Defining Alert Metadata Declare Required"
metadata:
  actual:
    adr_id: d4b10a0e-47eb-4f5c-9dd8-9ba9bcede0b3
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Schemas Defining Alert Metadata Declare Required

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for notification integration adapters constructing alert payloads for external dispatch, and schema definitions validating event severity, message details, and incident metadata.

### Rules

- **R-ZOD-001** MUST: Schemas defining alert metadata MUST declare required attributes using z.string and constrain incident status levels using z.enum validation.

### Verify

```bash
# Discover and run notification payload schema unit tests
# Discover and run static type analysis across notification integration packages
```

**Accept when:**
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>