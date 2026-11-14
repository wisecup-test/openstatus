---
version: 2
name: cross-cutting-converter-implementations-isolate-domain-a7f9
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Converter Implementations Isolate Domain Specific Transformations"
metadata:
  actual:
    adr_id: a7f996fb-8aee-44b1-8e73-ab6ae72595b9
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Converter Implementations Isolate Domain Specific Transformations

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for RPC route definitions, request validation routines, and data converters handling monitor domain operations.

### Rules

- **R-MON-001** SHOULD: Converter implementations SHOULD isolate domain-specific transformations into dedicated modules separated from transport routing logic.

### Verify

```bash
# Discover and run the project static analysis and type verification script
# Discover and run the test suite covering data converters and RPC route handlers
```

**Accept when:**
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static type checking, test execution, and peer code review.
</enforcement>