---
version: 2
name: cross-cutting-engineers-not-bypass-schema-parsing-e178
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: Engineers Not Bypass Schema Parsing Cast"
metadata:
  actual:
    adr_id: e178b914-b7ca-4d9a-839c-61f434148370
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: Engineers Not Bypass Schema Parsing Cast

Scope: applies to `packages/notifications/webhook/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Engineers MUST_NOT bypass schema parsing or cast unvalidated deserialized data directly to domain types at external integration boundaries.

### Verify

```bash
# Discover and execute the repository static analysis and type checking script to verify schema type adherence.
# Discover and run the project automated test suite to confirm schema validation passes on valid payloads and rejects malformed data.
```

**Accept when:**
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>