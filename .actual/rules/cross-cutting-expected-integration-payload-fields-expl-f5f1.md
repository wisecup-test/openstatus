---
version: 2
name: cross-cutting-expected-integration-payload-fields-expl-f5f1
description: "Adoption of Zod for Runtime Schema Validation: Expected Integration Payload Fields Explicitly Declared"
metadata:
  actual:
    adr_id: f5f1d090-5f80-43ec-b70b-7d95e734a883
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Expected Integration Payload Fields Explicitly Declared

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all boundary handlers, notification integration modules receiving untrusted external data payloads, and schema definition modules responsible for parsing external alert and notification structures.

### Rules

- **R-ZOD-001** MUST: All expected integration payload fields MUST be explicitly declared using Zod primitive validators such as z.string, enumerated domains via z.enum, or marked optional with the optional modifier.

### Verify

```bash
# Discover the project test runner configuration from the repository and execute test suites targeting schema validation modules.
# Discover the project type-checking configuration and execute the static type verification script across all integration packages.
# Discover the repository linting and schema validation check tasks to verify compliance with architectural constraints.
```

**Accept when:**
- All unit tests exercising schema parsing for valid and invalid payloads pass without failures.
- Static type checking completes with zero diagnostic errors across schema modules.
- All external payload ingestion points execute runtime schema validation prior to processing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>