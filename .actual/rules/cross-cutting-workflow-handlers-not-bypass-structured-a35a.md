---
version: 2
name: cross-cutting-workflow-handlers-not-bypass-structured-a35a
description: "Adoption of Effect Library for Workflow Services: Workflow Handlers Not Bypass Structured Effect"
metadata:
  actual:
    adr_id: a35a7585-4146-440b-9938-dec5cafd5d33
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

# Adoption of Effect Library for Workflow Services: Workflow Handlers Not Bypass Structured Effect

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** MUST_NOT: Workflow handlers MUST NOT bypass structured effect error-handling channels in favor of untyped unhandled exceptions or unmanaged promise rejections.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and automated type check suites executed during continuous integration.
</enforcement>