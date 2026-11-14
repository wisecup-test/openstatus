---
version: 2
name: cross-cutting-module-credentials-connection-configurat-b688
description: "Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Module Credentials Connection Configurations Analytics Client"
metadata:
  actual:
    adr_id: b6885325-7c6b-4d21-b3cf-f2682709e2e5
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

# Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Module Credentials Connection Configurations Analytics Client

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow tasks, cron jobs, and utility logging modules that transmit analytical telemetry or record audit events.

### Rules

- **R-ANL-001** SHOULD: Module credentials and connection configurations for the analytics client SHOULD be supplied via centralized environment configuration helpers.

### Verify

\`\`\`bash
# Discover the project task runner from the dependency manifest and run the architectural linting and dependency verification checks.
# Locate the repository test runner and execute the automated test suites governing workflow execution and audit logging to verify client integration.
\`\`\`

**Accept when:**
- All analytical operations, audit event records, and batch ingestions route strictly through the designated internal analytics module.
- The project verification scripts and test suites complete with zero errors and confirm no unauthorized direct network calls to the external analytics service.

<enforcement>
Claude Code MUST NOT skip or defer verification. All analytical telemetry and audit logging must conform to the centralized configuration and shared client requirements.
</enforcement>