---
version: 2
name: cross-cutting-boundary-clients-not-cast-unvalidated-00fd
description: "Zod Schema Validation for External Provider Payloads: Boundary Clients Not Cast Unvalidated External"
metadata:
  actual:
    adr_id: 00fd71b0-493b-4214-bb75-fd1e9cfd0707
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: Boundary Clients Not Cast Unvalidated External

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for external provider integration clients making network requests to third-party endpoints, and API response payload parsing and contract definition modules at the ingestion boundary.

### Rules

- **R-EXT-001** MUST_NOT: Boundary clients MUST NOT cast unvalidated external response objects directly to internal domain types without executing runtime schema validation.

### Verify

```bash
# Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
# Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.
```

**Accept when:**
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external client endpoints must invoke schema parse functions, verified by continuous integration test pipelines and peer code reviews.
</enforcement>