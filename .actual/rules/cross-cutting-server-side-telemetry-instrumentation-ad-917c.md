---
version: 2
name: cross-cutting-server-side-telemetry-instrumentation-ad-917c
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Server Side Telemetry Instrumentation Adopt Openpanel"
metadata:
  actual:
    adr_id: 917c813c-e212-459a-a020-cc96ab999b30
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Server Side Telemetry Instrumentation Adopt Openpanel

Scope: applies to `packages/analytics/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SS-001** MUST: Server-side telemetry instrumentation MUST adopt @openpanel/sdk through a dedicated server abstraction layer to handle event dispatching and tracking across backend services.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>