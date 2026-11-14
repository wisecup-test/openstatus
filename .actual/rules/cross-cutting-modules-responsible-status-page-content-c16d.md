---
version: 2
name: cross-cutting-modules-responsible-status-page-content-c16d
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Modules Responsible Status Page Content Generation"
metadata:
  actual:
    adr_id: c16d488b-79d5-4d64-a7ed-b22ea61e6a3c
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Modules Responsible Status Page Content Generation

Scope: applies to `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for modules responsible for status page content generation, presentation helpers, and routines evaluating component impact severity.

### Rules

- **R-STAT-001** MUST: Modules responsible for status page content generation and presentation helpers MUST import domain status vocabulary definitions and impact evaluation functions exclusively from the status-vocab module.

### Verify

```bash
# Discover and execute the project verification script to validate module import resolution and type consistency across status content generators.
# Discover and run the automated test suite covering markdown generator outputs and status evaluation functions.
```

**Accept when:**
- All status content generators import domain vocabulary and severity functions from status-vocab with zero compilation errors.
- Impact calculations correctly resolve cumulative component status through worstImpact in all automated test scenarios.
- No redundant status string definitions exist within presentation helpers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline running static analysis and type checks, and peer code review verifying that status vocabulary and impact evaluation use the centralized module.
</enforcement>