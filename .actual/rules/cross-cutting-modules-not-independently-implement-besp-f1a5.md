---
version: 2
name: cross-cutting-modules-not-independently-implement-besp-f1a5
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Not Independently Implement Bespoke Effective"
metadata:
  actual:
    adr_id: f1a59a8f-4571-402c-b261-5e9dea608f5e
    status: accepted
    candidate_group_id: 8a4fe0ef-90ea-4fed-b69f-f54f0e8f7fa9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Not Independently Implement Bespoke Effective

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules that calculate, display, or escalate service and component operational status (server-side escalation jobs, markdown report generators, and client-rendered status route views).

### Rules

- **R-EFF-001** MUST_NOT: Modules MUST NOT independently implement bespoke effective status derivation or component health aggregation heuristics that bypass the shared router contract.

### Verify

```bash
# Discover and execute the project dependency resolution validation script to confirm lock-artifact consistency.
# Discover and execute the repository static analysis and module boundary linter to verify import conformance with the shared router module.
# Discover and execute the automated test suite covering effective status calculation and consumer workflows.
```

**Accept when:**
- All status calculations and display surfaces derive state exclusively through the exported interfaces of the effective status router.
- Static boundary verification confirms zero direct or duplicated status evaluation implementations across consumer modules.
- The project verification suite passes without warnings or unresolved dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status calculation changes must be verified via static analysis, architectural boundary checks, and automated test suites. Pull requests violating this policy will be blocked.
</enforcement>