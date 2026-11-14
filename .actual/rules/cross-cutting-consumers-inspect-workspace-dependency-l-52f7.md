---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-l-52f7
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Consumers Inspect Workspace Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 52f7c071-4b07-4ecb-a39c-b96daaa6a95d
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Consumers Inspect Workspace Dependency Lock Artifact

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

### Rules

- **R-EXT-001** MUST: Consumers MUST inspect the workspace dependency lock artifact to determine and verify the exact resolved version of all project dependencies prior to implementing or modifying integration logic.
- **R-EXT-002** MUST: Verify that module imports in workflow packages resolve exclusively to the official public boundary of the adopted service package.
- **R-EXT-003** MUST: Ensure error handling strategies in asynchronous tasks explicitly capture and log exceptions propagated across the service boundary.

### Verify

```bash
# Discover and run the workspace static analysis script to verify module import constraints across workflow packages.
# Discover and run the workspace test runner to execute workflow integration tests against the service boundary.
```

**Accept when:**
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>