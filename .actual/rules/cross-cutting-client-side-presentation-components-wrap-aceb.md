---
version: 2
name: cross-cutting-client-side-presentation-components-wrap-aceb
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Client Side Presentation Components Wrap Query"
metadata:
  actual:
    adr_id: acebf6e5-924b-41ed-90fd-dca0986cb9e4
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

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Client Side Presentation Components Wrap Query

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** MAY: Client-side presentation components MAY wrap query responses from @openstatus/api/src/router/effective-status with local caching structures when managing historical time-series intervals.

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
Claude Code MUST NOT skip or defer verification. All status evaluation workflows must be verified via static analysis, architectural boundary checks, and automated test suites as mandated by the project enforcement policy.
</enforcement>