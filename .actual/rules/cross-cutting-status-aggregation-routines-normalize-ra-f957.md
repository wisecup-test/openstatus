---
version: 2
name: cross-cutting-status-aggregation-routines-normalize-ra-f957
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Status Aggregation Routines Normalize Raw Incident"
metadata:
  actual:
    adr_id: f9577bfb-9738-449f-b4ea-d82566b053fe
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Status Aggregation Routines Normalize Raw Incident

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for status dashboard pages, incident timeline views, service history displays, and data aggregation functions that map telemetry and incident records into daily status intervals.

### Rules

- **R-OPENSTATUS-001** SHOULD: Status aggregation routines SHOULD normalize raw incident timestamps and daily metric snapshots into the data contracts defined by the shared status types before passing them to presentation components.

### Verify

```bash
# Discover and execute the repository type-checking script across all workspace packages
# Discover and run the repository linter to detect unapproved local status type definitions
# Execute the automated test suite to verify correct rendering and aggregation of status timeline components
```

**Accept when:**
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status block and type contract implementations are verified via automated workspace type-checking, linting, and test execution.
</enforcement>