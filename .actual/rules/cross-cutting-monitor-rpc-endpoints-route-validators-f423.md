---
version: 2
name: cross-cutting-monitor-rpc-endpoints-route-validators-f423
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Monitor Rpc Endpoints Route Validators Data"
metadata:
  actual:
    adr_id: f4232a4d-8517-4e51-bc6d-c76877f14eb7
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Monitor Rpc Endpoints Route Validators Data

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all monitor RPC endpoints, route validators, and data converters handling monitor domain operations.

### Rules

- **R-MON-001** MUST: All monitor RPC endpoints, route validators, and data converters MUST adopt @openstatus/proto/monitor/v1 message definitions as their canonical contract interface.

### Verify

```bash
# Discover and run the project static analysis and type verification script
# Discover and run the test suite covering data converters and RPC route handlers
```

**Accept when:**
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All monitor RPC endpoints and converters must be validated via static analysis and test execution.
</enforcement>