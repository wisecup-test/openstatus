---
version: 2
name: cross-cutting-analytics-events-dispatched-through-open-9cbd
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Analytics Events Dispatched Through Openpanel Sdk"
metadata:
  actual:
    adr_id: 9cbd1861-62aa-47e3-9e1f-61421d97e08e
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Analytics Events Dispatched Through Openpanel Sdk

Scope: applies to `packages/analytics/src/**`.

These rules are ALWAYS ACTIVE for all server-side services and backend execution boundaries dispatching telemetry events, and centralized telemetry instrumentation modules configuring @openpanel/sdk.

### Rules

- **R-ANA-001** MUST: All analytics events dispatched through @openpanel/sdk MUST conform to predefined schemas defined in the centralized event catalog module.

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