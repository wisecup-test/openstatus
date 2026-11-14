---
version: 2
name: cross-cutting-modules-computing-rendering-escalating-s-4f86
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Computing Rendering Escalating Service Component"
metadata:
  actual:
    adr_id: 4f8611f3-f0f5-401c-a6e9-247983b70ee2
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

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Computing Rendering Escalating Service Component

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules that calculate, display, or escalate service and component operational status, including server-side escalation jobs, markdown report generators, and client-rendered status route views.

### Rules

- **R-EFF-001** MUST: All modules computing, rendering, or escalating service and component operational states MUST import and route status evaluations through `@openstatus/api/src/router/effective-status`.

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
Claude Code MUST NOT skip or defer verification. All status evaluations must be statically checked and verified via the project's verification suite.
</enforcement>