---
version: 2
name: cross-cutting-route-handlers-validators-not-construct-c496
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Route Handlers Validators Not Construct Hoc"
metadata:
  actual:
    adr_id: c496708b-15e7-49c7-9548-023389d9ea74
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Route Handlers Validators Not Construct Hoc

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MON-001** MUST_NOT: Route handlers and validators MUST NOT construct ad-hoc payload types or bypass @openstatus/proto/monitor/v1 definitions for monitor RPC operations.

### Verify

```bash
# Discover and run the project static analysis and type verification script to ensure all monitor RPC handlers conform to contract signatures.
# Discover and run the test suite covering data converters and RPC route handlers to validate message serialization and parsing.
```

**Accept when:**
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>