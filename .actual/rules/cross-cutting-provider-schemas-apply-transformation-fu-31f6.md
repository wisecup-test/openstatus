---
version: 2
name: cross-cutting-provider-schemas-apply-transformation-fu-31f6
description: "Zod Schema Validation for External Provider Payloads: Provider Schemas Apply Transformation Functions Handle"
metadata:
  actual:
    adr_id: 31f68206-31be-4470-a0b0-2c656a768d88
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: Provider Schemas Apply Transformation Functions Handle

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROVIDER-001** SHOULD: Provider schemas SHOULD apply transformation functions to handle missing or optional values at the ingestion boundary rather than propagating nullable states downstream.

### Verify

```bash
# Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
# Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.
```

**Accept when:**
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external provider payloads must be validated and transformed via schemas at the ingestion boundary.
</enforcement>