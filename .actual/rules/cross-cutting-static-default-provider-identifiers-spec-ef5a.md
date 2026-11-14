---
version: 2
name: cross-cutting-static-default-provider-identifiers-spec-ef5a
description: "Zod Schema Definition for Notification Payloads: Static Default Provider Identifiers Specified Schema"
metadata:
  actual:
    adr_id: ef5a76d5-0200-4a87-b709-59037fc554cc
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Static Default Provider Identifiers Specified Schema

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for notification integration adapters constructing alert payloads for external dispatch and schema definitions validating event severity, message details, and incident metadata.

### Rules

- **R-ZOD-001** SHOULD: Static default provider identifiers MUST be specified using schema default assignment methods to guarantee consistent event provenance.

### Verify

```bash
# Discover the project test runner and execute notification payload schema unit tests
# Discover the project static type analysis runner and execute type validation across notification integration packages
```

**Accept when:**
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suite execution and static type analysis checks verify payload schemas and inferred type alignment.
</enforcement>