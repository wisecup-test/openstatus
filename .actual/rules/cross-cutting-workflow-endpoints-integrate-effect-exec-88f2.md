---
version: 2
name: cross-cutting-workflow-endpoints-integrate-effect-exec-88f2
description: "Adoption of Effect Library for Workflow Services: Workflow Endpoints Integrate Effect Execution Pipelines"
metadata:
  actual:
    adr_id: 88f2a6c0-305c-44a6-9fc7-79b4a91751a9
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

# Adoption of Effect Library for Workflow Services: Workflow Endpoints Integrate Effect Execution Pipelines

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** MAY: Workflow endpoints MAY integrate effect execution pipelines with routing handlers and scheduled trigger middleware.

### Verify

```bash
echo "Discover and execute the project verification script defined in the dependency manifest"
echo "Discover and run static type checking to verify compliance of effect pipeline signatures"
echo "Inspect lock resolution artifacts to verify the resolved version of effect matches documented APIs"
```

**Accept when:**
- The project type verification and static analysis checks pass with zero diagnostic errors across workflow modules.
- All asynchronous workflow handlers return managed effect computations without unhandled promise rejections.
- The resolved version of effect in the repository lock artifact matches the API surface used in workflow code.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>