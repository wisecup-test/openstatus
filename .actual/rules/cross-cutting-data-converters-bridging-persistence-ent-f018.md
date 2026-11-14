---
version: 2
name: cross-cutting-data-converters-bridging-persistence-ent-f018
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Data Converters Bridging Persistence Entities Transport"
metadata:
  actual:
    adr_id: f018fb53-a371-4a6c-8f23-cc4e75e437ff
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Data Converters Bridging Persistence Entities Transport

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MON-001** MUST: Data converters bridging persistence entities and transport payloads MUST perform explicit bidirectional conversion to and from @openstatus/proto/monitor/v1 structures.

### Verify

```bash
# Discover and run the project static analysis and type verification script to ensure all monitor RPC handlers conform to contract signatures.
# Discover and run the test suite covering data converters and RPC route handlers to validate message serialization and parsing.
```

**Accept when:**
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detecting contract type violations, and peer code review verifies converter and handler imports for contract compliance.
</enforcement>