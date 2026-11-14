---
version: 2
name: cross-cutting-engineers-inspect-project-lock-artifact-b1e5
description: "Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Engineers Inspect Project Lock Artifact Resolve"
metadata:
  actual:
    adr_id: b1e5d01b-5d72-4080-920e-b097bb708318
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

# Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Engineers Inspect Project Lock Artifact Resolve

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow tasks, cron jobs, and utility logging modules that transmit analytical telemetry or record audit events.

### Rules

- **R-TIN-001** MUST: Engineers MUST inspect the project lock artifact to resolve and verify the exact resolved version of the analytics package before implementing or modifying analytical operations.

### Verify

```bash
# Discover the project task runner from the dependency manifest and run the architectural linting and dependency verification checks.
# Locate the repository test runner and execute the automated test suites governing workflow execution and audit logging to verify client integration.
```

**Accept when:**
- All analytical operations, audit event records, and batch ingestions route strictly through the designated internal analytics module.
- The project verification scripts and test suites complete with zero errors and confirm no unauthorized direct network calls to the external analytics service.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer review enforce these policies.
</enforcement>