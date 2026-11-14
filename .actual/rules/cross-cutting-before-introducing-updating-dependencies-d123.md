---
version: 2
name: cross-cutting-before-introducing-updating-dependencies-d123
description: "Adoption of Effect Library for Workflow Services: Before Introducing Updating Dependencies Effect Developers"
metadata:
  actual:
    adr_id: d123484d-134d-4e25-9251-e67641a8ccab
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

# Adoption of Effect Library for Workflow Services: Before Introducing Updating Dependencies Effect Developers

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for asynchronous workflow task handlers, scheduled cron job processors, and background incident and alerting dispatchers.

### Rules

- **R-EFF-001** MUST: Before introducing or updating dependencies on effect, developers MUST inspect the repository lock artifact to determine and verify the exact resolved version against official documentation.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>