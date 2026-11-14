---
version: 2
name: cross-cutting-external-http-response-payloads-received-a88c
description: "Zod Schema Validation for External Provider Payloads: External Http Response Payloads Received Across"
metadata:
  actual:
    adr_id: a88cd12a-5ebf-4e95-9310-a05753b01f7d
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: External Http Response Payloads Received Across

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All external HTTP response payloads received across integration boundaries MUST be validated and parsed using Zod schemas via runtime parse operations before being consumed by internal domain services.

### Verify

```bash
# Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
# Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.
```

**Accept when:**
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>