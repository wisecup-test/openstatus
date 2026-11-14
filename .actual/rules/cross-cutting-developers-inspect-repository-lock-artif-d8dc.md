---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-d8dc
description: "Zod Schema Validation for External API Client Boundaries: Developers Inspect Repository Lock Artifact Identify"
metadata:
  actual:
    adr_id: d8dc2fdf-9a75-4ae3-b60d-ecfa0867c704
    status: accepted
    candidate_group_id: 2a48e15c-71e7-4ab2-94a0-84daf33ebd71
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External API Client Boundaries: Developers Inspect Repository Lock Artifact Identify

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for external API client integrations, boundary modules responsible for ingesting, adapting, or importing remote provider data, and files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Developers MUST inspect the repository lock artifact to identify the exact resolved version of the schema validation library and verify API compatibility before implementing schema definitions.

### Verify

```bash
# Discover and run the repository test runner targeting external integration client suites to verify payload parsing and error handling.
# Discover and execute the repository static analysis linter to confirm that external client fetch responses pass through schema validation.
```

**Accept when:**
- All external client integration tests pass, confirming schema validation executes on network response payloads.
- No unvalidated external response payloads are exposed directly through provider client interfaces.
- Schema parsing rejections yield structured validation errors rather than unhandled raw network exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>