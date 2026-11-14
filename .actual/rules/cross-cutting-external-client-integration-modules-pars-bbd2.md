---
version: 2
name: cross-cutting-external-client-integration-modules-pars-bbd2
description: "Zod Schema Validation for External API Client Boundaries: External Client Integration Modules Parse Http"
metadata:
  actual:
    adr_id: bbd264bd-e203-46fd-9b30-40c2f885be1c
    status: accepted
    candidate_group_id: 2a48e15c-71e7-4ab2-94a0-84daf33ebd71
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External API Client Boundaries: External Client Integration Modules Parse Http

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for external API client integration modules that ingest data from third-party or remote HTTP endpoints.

### Rules

- **R-ZOD-001** MUST: External API client integration modules MUST parse all external HTTP response payloads through Zod schemas using schema.parse before returning or transforming data for internal consumption.

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
Claude Code MUST NOT skip or defer verification. All external HTTP client responses must be verified against Zod schemas prior to internal consumption.
</enforcement>