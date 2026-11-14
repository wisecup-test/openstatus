---
version: 2
name: cross-cutting-analytical-event-queries-monitor-telemet-22eb
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Analytical Event Queries Monitor Telemetry Reads"
metadata:
  actual:
    adr_id: 22ebe40c-b180-4337-82e1-bfe97ba91633
    status: accepted
    candidate_group_id: a1c4968b-fba3-4688-8ea7-a915cddc7814
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/monitor/**
      - src/content/**
      - src/lib/**
---

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Analytical Event Queries Monitor Telemetry Reads

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

### Rules

- **R-TB-001** MUST: All analytical event queries, monitor telemetry reads, and time-series reporting data fetching MUST be conducted using @openstatus/tinybird client interfaces.

### Verify

```bash
# Discover and run the project dependency verification command
# Discover and run the project static type check script
# Discover and run the project test suite
```

**Accept when:**
- All analytical and time-series reporting queries compile against the verified @openstatus/tinybird interface without type or contract mismatches.
- The project test suite passes with zero regressions in escalation reporting and status markdown generation.
- Direct transactional database queries for time-series event metrics remain absent across application service layers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>