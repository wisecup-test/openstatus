---
version: 2
name: cross-cutting-error-states-originating-openstatus-tiny-45c6
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Error States Originating Openstatus Tinybird Operations"
metadata:
  actual:
    adr_id: 45c6b8df-71e3-45c3-bc0e-9c20dd97d6d7
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

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Error States Originating Openstatus Tinybird Operations

Scope: applies to `apps/web/src/app/api/og/monitor/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TB-001** MUST: Error states originating from @openstatus/tinybird operations MUST be handled with structured diagnostic logging without terminating critical caller workflows.

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
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import boundaries, and peer code reviews verify adherence.
</enforcement>