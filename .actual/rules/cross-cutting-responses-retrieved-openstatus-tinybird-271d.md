---
version: 2
name: cross-cutting-responses-retrieved-openstatus-tinybird-271d
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Responses Retrieved Openstatus Tinybird Cacheable Presentation"
metadata:
  actual:
    adr_id: 271d7c5f-b7a6-400c-bde4-fb8696db7a68
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

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Responses Retrieved Openstatus Tinybird Cacheable Presentation

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

### Rules

- **R-OT-001** SHOULD: Responses retrieved from @openstatus/tinybird in cacheable presentation or markdown rendering paths SHOULD pass through application-level caching layers to prevent redundant analytical queries.

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
Claude Code MUST NOT skip or defer verification. Automated static analysis and module import boundaries must be verified during continuous integration.
</enforcement>