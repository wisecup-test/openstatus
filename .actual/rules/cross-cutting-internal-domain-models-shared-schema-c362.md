---
version: 2
name: cross-cutting-internal-domain-models-shared-schema-c362
description: "Adoption of Zod for Runtime Schema Validation: Internal Domain Models Shared Schema Entities"
metadata:
  actual:
    adr_id: c362231e-7ca7-4504-a67d-110bd770efba
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Internal Domain Models Shared Schema Entities

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all boundary handlers, notification integration modules receiving untrusted external data payloads, and schema definition modules responsible for parsing external alert and notification structures.

### Rules

- **R-ZOD-001** SHOULD: Internal domain models and shared schema entities SHOULD derive their static TypeScript types directly from the Zod schemas using schema inference rather than maintaining duplicate type declarations.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines enforce type checking and schema unit test suites, and peer code reviews verify that incoming integration payloads are validated through Zod schemas.
</enforcement>