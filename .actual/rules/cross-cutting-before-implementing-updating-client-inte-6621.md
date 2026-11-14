---
version: 2
name: cross-cutting-before-implementing-updating-client-inte-6621
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Before Implementing Updating Client Integrations Openstatus"
metadata:
  actual:
    adr_id: 6621f946-7618-4700-a3b5-f674d66e5edd
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

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Before Implementing Updating Client Integrations Openstatus

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

### Rules

- **R-TB-001** MUST: Before implementing or updating client integrations with @openstatus/tinybird, the consumer MUST discover the repository dependency manifest and resolution lock artifact to verify the exact resolved version.
- **R-TB-002** MUST: Discover the repository dependency manifest, identify the build tool, inspect the resolution lock artifact, and look up official documentation for the exact resolved version before writing code that uses a versioned library.
- **R-TB-003** MUST: Centralize client initialization in a shared internal module that extracts credentials and endpoint configuration from discovered environment variables.
- **R-TB-004** MUST: Implement defensive error handling around analytical calls to ensure monitoring and notification workflows degrade gracefully if remote analytics endpoints become unreachable.

### Verify

```bash
# Discover and execute the repository dependency verification command to ensure package resolution is valid
# Discover and execute the project static type check script to verify @openstatus/tinybird type contracts
# Discover and run the project test suite to verify escalation reporting and status query workflows
```

**Accept when:**
- All analytical and time-series reporting queries compile against the verified @openstatus/tinybird interface without type or contract mismatches.
- The project test suite passes with zero regressions in escalation reporting and status markdown generation.
- Direct transactional database queries for time-series event metrics remain absent across application service layers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import boundaries, and peer code reviews enforce compliance.
</enforcement>