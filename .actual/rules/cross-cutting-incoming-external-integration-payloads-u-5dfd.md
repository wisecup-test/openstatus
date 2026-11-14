---
version: 2
name: cross-cutting-incoming-external-integration-payloads-u-5dfd
description: "Adoption of Zod for Runtime Schema Validation: Incoming External Integration Payloads Untrusted Boundary"
metadata:
  actual:
    adr_id: 5dfdd344-18bc-4e63-a1e2-ccfa1b5a5090
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Incoming External Integration Payloads Untrusted Boundary

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all boundary handlers, notification integration modules, and schema definition modules receiving untrusted external data payloads.

### Rules

- **R-ZOD-001** MUST: Incoming external integration payloads and untrusted boundary data MUST be validated and parsed using Zod schemas constructed via z.object definitions.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipelines, peer code reviews, static analysis, and build checks.
</enforcement>