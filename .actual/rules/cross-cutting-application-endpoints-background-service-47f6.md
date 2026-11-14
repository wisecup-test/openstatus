---
version: 2
name: cross-cutting-application-endpoints-background-service-47f6
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Application Endpoints Background Service Reporters Not"
metadata:
  actual:
    adr_id: 47f669c3-e4b5-427d-afa0-09a34a3a6720
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

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Application Endpoints Background Service Reporters Not

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

### Rules

- **R-TB-001** MUST_NOT: Application endpoints and background service reporters MUST NOT query underlying transactional relational databases for high-cardinality time-series event metrics managed by @openstatus/tinybird.

### Verify

```bash
# Discover and execute the project dependency verification command
# Discover and execute the project static type check script
# Discover and run the project test suite
```

**Accept when:**
- All analytical and time-series reporting queries compile against the verified @openstatus/tinybird interface without type or contract mismatches.
- The project test suite passes with zero regressions in escalation reporting and status markdown generation.
- Direct transactional database queries for time-series event metrics remain absent across application service layers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, peer code reviews, and build checks enforce these architectural rules.
</enforcement>