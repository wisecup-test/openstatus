---
version: 2
name: cross-cutting-external-client-requests-pass-explicit-86a0
description: "Zod Schema Validation for External API Client Boundaries: External Client Requests Pass Explicit Authentication"
metadata:
  actual:
    adr_id: 86a0d4ea-ae85-42a6-b36a-ae118de287f2
    status: accepted
    candidate_group_id: 2a48e15c-71e7-4ab2-94a0-84daf33ebd71
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External API Client Boundaries: External Client Requests Pass Explicit Authentication

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for external API client integrations, boundary modules responsible for remote provider data ingestion, and third-party HTTP fetching layers.

### Rules

- **R-ZOD-001** SHOULD: External API client requests SHOULD pass explicit authentication and content negotiation headers alongside schema parsing boundaries.

### Verify

```bash
# Discover and run the repository test runner targeting external integration client suites
npm test -- --grep "integration|client|boundary"
# Discover and execute the repository static analysis linter
npm run lint
```

**Accept when:**
- All external client integration tests pass, confirming schema validation executes on network response payloads.
- No unvalidated external response payloads are exposed directly through provider client interfaces.
- Schema parsing rejections yield structured validation errors rather than unhandled raw network exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external API client code must implement explicit authentication headers and strict Zod schema validation on all incoming remote responses.
</enforcement>