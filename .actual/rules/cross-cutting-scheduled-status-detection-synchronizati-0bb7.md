---
version: 2
name: cross-cutting-scheduled-status-detection-synchronizati-0bb7
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Scheduled Status Detection Synchronization Tasks Delegate"
metadata:
  actual:
    adr_id: 0bb7f898-901a-4345-b35e-f11b003a7820
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Scheduled Status Detection Synchronization Tasks Delegate

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

### Rules

- **R-EXT-001** SHOULD: Scheduled status detection and synchronization tasks SHOULD delegate upstream provider responses to service boundary methods rather than embedding custom persistence transformation logic within workflow definitions.

### Verify

```bash
# Discover and run the workspace static analysis script to verify module import constraints across workflow packages.
# Discover and run the workspace test runner to execute workflow integration tests against the service boundary.
```

**Accept when:**
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis, import boundary checks, and automated peer code review.
</enforcement>