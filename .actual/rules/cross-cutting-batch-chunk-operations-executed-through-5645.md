---
version: 2
name: cross-cutting-batch-chunk-operations-executed-through-5645
description: "Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Batch Chunk Operations Executed Through Analytics"
metadata:
  actual:
    adr_id: 56450cae-0d84-4e84-adc1-741c25cd762d
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

# Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Batch Chunk Operations Executed Through Analytics

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow tasks, cron jobs, and utility logging modules that transmit analytical telemetry or record audit events.

### Rules

- **R-TNY-001** SHOULD: Batch and chunk operations executed through the analytics client SHOULD isolate failure handling per chunk, logging structured error diagnostics while allowing unaffected operations to proceed.

### Verify

```bash
# Discover the project task runner from the dependency manifest and run the architectural linting and dependency verification checks.
# Locate the repository test runner and execute the automated test suites governing workflow execution and audit logging to verify client integration.
```

**Accept when:**
- All analytical operations, audit event records, and batch ingestions route strictly through the designated internal analytics module.
- The project verification scripts and test suites complete with zero errors and confirm no unauthorized direct network calls to the external analytics service.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>