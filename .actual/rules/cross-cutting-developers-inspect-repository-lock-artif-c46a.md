---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-c46a
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Developers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: c46a6ed2-7a80-4659-87d1-107c44b8247b
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Developers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for RPC route definitions, request validation routines, and data converters handling monitor domain operations, and module boundaries adapting between network transport serialization and internal domain models.

### Rules

- **R-MON-001** MUST: Developers MUST inspect the repository lock artifact to determine the exact resolved version of the contract dependency before implementation.

### Verify

```bash
# Discover and run project static analysis, type verification, and test suite covering data converters and RPC route handlers
```

**Accept when:**
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

<enforcement>
Verification via automated static type checking, test execution, and peer code review is mandatory. Pull requests introducing direct persistence model leaks or bypassing contract schemas are rejected at code review, and continuous integration builds fail upon detecting contract type violations.
</enforcement>