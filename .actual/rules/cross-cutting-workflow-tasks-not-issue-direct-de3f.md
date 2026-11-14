---
version: 2
name: cross-cutting-workflow-tasks-not-issue-direct-de3f
description: "Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Workflow Tasks Not Issue Direct Unmanaged"
metadata:
  actual:
    adr_id: de3ffb48-e478-47e4-a10b-adfcff5b68ac
    status: accepted
    candidate_group_id: c8e7cec8-7a72-4a93-ab64-16c9cff4fc8f
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/utils/**
---

# Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Workflow Tasks Not Issue Direct Unmanaged

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow tasks, cron jobs, and utility logging modules that transmit analytical telemetry or record audit events.

### Rules

- **R-ANL-001** MUST_NOT: Workflow tasks MUST_NOT issue direct unmanaged HTTP requests to the external analytics service endpoints.
- **R-ANL-002** MANDATORY: Discover the dependency manifest in the repo to identify the build tool, inspect the lock or resolution artifact for the exact resolved version, and check official documentation for that exact version before writing code that uses a versioned library.
- **R-ANL-003** MANDATORY: Instantiate or consume the analytics client using environment variables provided through the centralized configuration module.
- **R-ANL-004** MANDATORY: Ensure chunked batch operations encapsulate failures in structured try-catch blocks and log warning messages with contextual failure metadata rather than terminating the entire job.

### Verify

```bash
# Discover the project task runner from the dependency manifest and run architectural linting and dependency verification checks
# Locate the repository test runner and execute automated test suites governing workflow execution and audit logging
```

**Accept when:**
- All analytical operations, audit event records, and batch ingestions route strictly through the designated internal analytics module.
- The project verification scripts and test suites complete with zero errors and confirm no unauthorized direct network calls to the external analytics service.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis checks and peer reviews strictly enforce these rules.
</enforcement>