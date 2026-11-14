---
version: 2
name: cross-cutting-consumers-discover-repository-dependency-d11a
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Consumers Discover Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: d11a4eae-7fe5-4ae3-a60e-bf928149c7d1
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

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Consumers Discover Repository Dependency Manifest Resolution

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** MUST: Consumers MUST discover the repository dependency manifest and resolution artifact to verify the exact resolved version and exported contract of the adopted module prior to implementation.

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
Claude Code MUST NOT skip or defer verification. All status evaluations must consume the shared router module and pass static analysis and automated test suites.
</enforcement>