---
version: 2
name: cross-cutting-telemetry-dispatch-operations-isolated-c-fe7a
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Telemetry Dispatch Operations Isolated Critical Execution"
metadata:
  actual:
    adr_id: fe7a408e-15ec-47f3-af17-67ad009a1739
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Telemetry Dispatch Operations Isolated Critical Execution

Scope: applies to `packages/analytics/src/**`.

These rules are ALWAYS ACTIVE for server-side services, backend execution boundaries, and centralized telemetry instrumentation modules configuring `@openpanel/sdk`.

### Rules

- **R-TEL-001** SHOULD: Telemetry dispatch operations SHOULD be isolated from critical execution flows such that network failures or telemetry errors do not disrupt primary server operations.

### Verify

```bash
# Discover and run the repository test runner to execute server analytics unit and integration tests.
# Discover and execute the repository type-check script to verify compliance with analytics event schemas.
# Discover and execute the repository dependency validation task to ensure lockfile synchronization.
```

**Accept when:**
- All server analytics tests pass with successful client instantiation and event dispatch simulation.
- Static type verification succeeds without errors across analytics event payloads.
- Dependency lockfile validation completes without uncommitted version discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>