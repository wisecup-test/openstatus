---
version: 2
name: cross-cutting-workflow-executions-maintain-localized-m-4639
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Workflow Executions Maintain Localized Memory Lookup"
metadata:
  actual:
    adr_id: 463995a8-e8d2-4496-9bea-4f3c66e1eb8e
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Workflow Executions Maintain Localized Memory Lookup

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

### Rules

- **R-EXT-001** MAY: Workflow executions MAY maintain localized in-memory lookup caches to eliminate redundant calls to @openstatus/services/external-service during a single scheduled execution run.

### Verify

```bash
# Discover and run the workspace static analysis script to verify module import constraints across workflow packages
# Discover and run the workspace test runner to execute workflow integration tests against the service boundary
```

**Accept when:**
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via static analysis, import boundary checks, and mandatory peer code review in automated integration pipelines.
</enforcement>