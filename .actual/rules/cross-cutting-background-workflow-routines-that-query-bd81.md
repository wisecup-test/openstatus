---
version: 2
name: cross-cutting-background-workflow-routines-that-query-bd81
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Background Workflow Routines That Query Mutate"
metadata:
  actual:
    adr_id: bd819363-7924-4c36-930c-e62e5c0692e2
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Background Workflow Routines That Query Mutate

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

### Rules

- **R-EXT-001** MUST: Background workflow routines that query or mutate external service records MUST route all external service entity interactions through the @openstatus/services/external-service module boundary.

### Verify

```bash
# Discover and run the workspace static analysis script to verify module import constraints across workflow packages.
# Discover and run the workspace test runner to execute workflow integration tests against the service boundary.
```

**Accept when:**
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and integration pipelines.
</enforcement>