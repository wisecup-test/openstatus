---
version: 2
name: cross-cutting-client-instances-configuration-openstatu-4e2a
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Client Instances Configuration Openstatus Tinybird Centralized"
metadata:
  actual:
    adr_id: 4e2abb98-05bc-4f1b-bd37-64c9120005f1
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

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Client Instances Configuration Openstatus Tinybird Centralized

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

### Rules

- **R-TB-001** SHOULD: Client instances and configuration for @openstatus/tinybird SHOULD be centralized within a shared library module rather than instantiated ad hoc in separate service boundaries.
- **R-TB-002** MANDATORY: Execute the lock-version grounding sequence (discover dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist) before writing code that uses a versioned library.
- **R-TB-003** MANDATORY: Wrap analytical queries in defensive error handling and employ presentation cache layers to serve stale content during transient failures.

### Verify

```bash
# Discover and execute repository dependency verification command
# Discover and execute project static type check script
# Discover and run project test suite
```

**Accept when:**
- All analytical and time-series reporting queries compile against the verified @openstatus/tinybird interface without type or contract mismatches.
- The project test suite passes with zero regressions in escalation reporting and status markdown generation.
- Direct transactional database queries for time-series event metrics remain absent across application service layers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import boundaries, and peer code reviews verify compliance; unapproved ad hoc query mechanisms or uncentralized analytical clients are blocked.
</enforcement>