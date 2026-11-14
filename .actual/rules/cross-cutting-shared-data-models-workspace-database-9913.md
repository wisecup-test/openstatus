---
version: 2
name: cross-cutting-shared-data-models-workspace-database-9913
description: "Adoption of Zod for Runtime Schema Validation: Shared Data Models Workspace Database Schema"
metadata:
  actual:
    adr_id: 99133aff-09be-4ff9-babb-dddf82c30d0d
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Shared Data Models Workspace Database Schema

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all boundary handlers, notification integration modules receiving untrusted external data payloads, and schema definition modules responsible for parsing external alert and notification structures.

### Rules

- **R-ZOD-001** SHOULD: Shared data models from workspace database schema packages SHOULD be composed with notification integration schemas to ensure consistency across data persistence boundaries.
- **R-ZOD-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY: Prior to writing code using a versioned library, execute lock-version grounding: find the dependency manifest, identify the build tool, inspect the repository lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm APIs exist, and re-verify per dependency at point of use.
- **R-ZOD-004** MANDATORY: Construct schema definitions using z.object composed with specific field validators and enum constraints to reflect required payload structures.
- **R-ZOD-005** MANDATORY: Export inferred static types alongside schema definitions to ensure downstream consumers use strongly typed representations of parsed payloads.
- **R-ZOD-006** MANDATORY: Handle parse errors gracefully at the ingress boundary to return structured error responses without leaking internal stack traces.

### Verify

```bash
# Discover and execute the project test runner configuration targeting schema validation modules
# Discover and execute the project type-checking configuration across all integration packages
# Discover and execute repository linting and schema validation check tasks
```

**Accept when:**
- All unit tests exercising schema parsing for valid and invalid payloads pass without failures.
- Static type checking completes with zero diagnostic errors across schema modules.
- All external payload ingestion points execute runtime schema validation prior to processing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code reviews verify validation compliance; pull requests bypassing runtime validation or using unvalidated type assertions must be blocked.
</enforcement>