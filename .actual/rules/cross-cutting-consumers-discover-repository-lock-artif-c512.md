---
version: 2
name: cross-cutting-consumers-discover-repository-lock-artif-c512
description: "Zod Schema Definition for Notification Payloads: Consumers Discover Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: c51286bd-dff2-4214-bf88-cce47255212c
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Consumers Discover Repository Lock Artifact Verify

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for notification integration modules interfacing with core database schemas while constructing provider-specific incident alert structures.

### Rules

- **R-ZOD-001** MUST: Consumers MUST discover the repository lock artifact and verify the exact resolved version of the schema validation library before introducing or modifying schema declarations.

### Verify

```bash
# Discover the project test runner and execute notification payload schema unit tests
# Discover the project static type analysis runner and execute type validation across notification integration packages
```

**Accept when:**
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>