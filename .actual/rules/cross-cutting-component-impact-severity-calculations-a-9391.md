---
version: 2
name: cross-cutting-component-impact-severity-calculations-a-9391
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Component Impact Severity Calculations Across Aggregated"
metadata:
  actual:
    adr_id: 9391790a-9a14-4f92-8a96-cce12001d4b8
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Component Impact Severity Calculations Across Aggregated

Scope: applies to `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STV-001** MUST: Component impact severity calculations across aggregated status items MUST utilize the worstImpact evaluation function provided by the status-vocab module rather than reimplementing precedence logic.

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
Claude Code MUST NOT skip or defer verification. All status vocabulary and impact evaluation must use the centralized module.
</enforcement>