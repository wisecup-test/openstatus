---
version: 2
name: cross-cutting-optional-nested-payload-metadata-declare-e92a
description: "Zod Schema Definition for Notification Payloads: Optional Nested Payload Metadata Declared Via"
metadata:
  actual:
    adr_id: e92a76bc-f4c2-4f57-9e1e-a1f0007531b9
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Optional Nested Payload Metadata Declared Via

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for all notification integration adapters constructing alert payloads and schema definitions validating event severity, message details, and incident metadata.

### Rules

- **R-ZOD-001** SHOULD: Optional nested payload metadata MUST be declared via z.object and chained with optional modifiers rather than accepting unconstrained object values.

### Verify

```bash
# Discover the repository test runner from project configuration and execute notification payload schema unit tests.
# Discover the project static type analysis runner and execute type validation across notification integration packages.
```

**Accept when:**
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>