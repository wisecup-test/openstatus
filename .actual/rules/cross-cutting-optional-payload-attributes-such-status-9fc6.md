---
version: 2
name: cross-cutting-optional-payload-attributes-such-status-9fc6
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: Optional Payload Attributes Such Status Codes"
metadata:
  actual:
    adr_id: 9fc6f380-6eb6-44d9-8941-3f89819b4f31
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: Optional Payload Attributes Such Status Codes

Scope: applies to `packages/notifications/webhook/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Optional payload attributes such as status codes, latency metrics, and error descriptions SHOULD be defined with explicit optional schema modifiers rather than permitting unvalidated structural properties.

### Verify

```bash
# Discover and execute the repository static analysis and type checking script
# Discover and run the project automated test suite
```

**Accept when:**
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>