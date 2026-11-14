---
version: 2
name: cross-cutting-telemetry-client-initialization-derive-r-33b1
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Telemetry Client Initialization Derive Runtime Configuration"
metadata:
  actual:
    adr_id: 33b1d80a-90bc-469c-9004-0af97a8d088e
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Telemetry Client Initialization Derive Runtime Configuration

Scope: applies to `packages/analytics/src/**`.

These rules are ALWAYS ACTIVE for server-side services and backend execution boundaries dispatching telemetry events, and centralized telemetry instrumentation modules configuring @openpanel/sdk.

### Rules

- **R-ANALYTICS-001** MUST: Telemetry client initialization MUST derive runtime configuration and deployment context indicators exclusively through centralized environment modules rather than direct inline environment property access.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute test suites and static type validation, and peer code review audits server-side telemetry invocations and event definitions.
</enforcement>