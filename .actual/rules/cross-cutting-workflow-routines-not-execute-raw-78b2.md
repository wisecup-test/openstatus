---
version: 2
name: cross-cutting-workflow-routines-not-execute-raw-78b2
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Workflow Routines Not Execute Raw Persistence"
metadata:
  actual:
    adr_id: 78b2286e-58df-4401-b23e-de0045bd9597
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Workflow Routines Not Execute Raw Persistence

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

### Rules

- **R-EXT-001** MUST_NOT: Workflow routines MUST NOT execute raw persistence queries or interact directly with underlying storage tables for entities managed by @openstatus/services/external-service.

### Verify

```bash
# Discover and run the workspace static analysis script to verify module import constraints across workflow packages.
# Discover and run the workspace test runner to execute workflow integration tests against the service boundary.
```

**Accept when:**
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and import boundary rules evaluated in automated integration pipelines, alongside mandatory peer code review.
</enforcement>