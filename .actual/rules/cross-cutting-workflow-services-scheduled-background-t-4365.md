---
version: 2
name: cross-cutting-workflow-services-scheduled-background-t-4365
description: "Adoption of Effect Library for Workflow Services: Workflow Services Scheduled Background Tasks Adopt"
metadata:
  actual:
    adr_id: 4365c41d-88e5-43f2-a588-1cd76b976fef
    status: accepted
    candidate_group_id: 8f23a55a-0b96-4d0e-88b7-ec6fbdab8372
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
---

# Adoption of Effect Library for Workflow Services: Workflow Services Scheduled Background Tasks Adopt

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** MUST: Workflow services and scheduled background tasks MUST adopt effect as the core functional runtime library for asynchronous orchestration and structured error handling.

### Verify

```bash
# Discover and execute the project verification script defined in the dependency manifest
# Discover and run static type checking to verify compliance of effect pipeline signatures
# Inspect lock resolution artifacts to verify the resolved version of effect matches documented APIs
```

**Accept when:**
- The project type verification and static analysis checks pass with zero diagnostic errors across workflow modules.
- All asynchronous workflow handlers return managed effect computations without unhandled promise rejections.
- The resolved version of effect in the repository lock artifact matches the API surface used in workflow code.

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow modules and scheduled tasks must use the effect runtime library with verified lock-version grounding, verified via static analysis and automated type check suites.
</enforcement>