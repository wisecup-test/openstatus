---
version: 2
name: cross-cutting-handlers-not-bypass-zod-schema-8986
description: "Adoption of Zod for Runtime Schema Validation: Handlers Not Bypass Zod Schema Parsing"
metadata:
  actual:
    adr_id: 8986cb1f-a7d5-4808-bbac-6604ac87c594
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Handlers Not Bypass Zod Schema Parsing

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all boundary handlers, notification integration modules, and schema definition modules receiving untrusted external data payloads.

### Rules

- **R-ZOD-001** MUST_NOT: Handlers MUST_NOT bypass Zod schema parsing or cast raw integration request bodies to static types without executing runtime validation.

### Verify

```bash
# Discover and execute test suites targeting schema validation modules
# Discover and execute the static type verification script across all integration packages
# Discover and execute repository linting and schema validation check tasks
```

**Accept when:**
- All unit tests exercising schema parsing for valid and invalid payloads pass without failures.
- Static type checking completes with zero diagnostic errors across schema modules.
- All external payload ingestion points execute runtime schema validation prior to processing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>