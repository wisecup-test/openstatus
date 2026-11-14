---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-caa3
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Consumers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: caa36032-42f2-48f1-8e3e-7d1f4c867c08
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Consumers Inspect Repository Dependency Lock Artifact

Scope: applies to `packages/analytics/src/**`.

These rules are ALWAYS ACTIVE for server-side services, backend execution boundaries, and centralized telemetry instrumentation modules configuring @openpanel/sdk.

### Rules

- **R-ANALYTICS-001** MUST: Consumers MUST inspect the repository dependency lock artifact to determine and verify the exact resolved version of @openpanel/sdk prior to implementation.

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