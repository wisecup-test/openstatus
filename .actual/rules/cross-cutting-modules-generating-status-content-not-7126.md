---
version: 2
name: cross-cutting-modules-generating-status-content-not-7126
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Modules Generating Status Content Not Define"
metadata:
  actual:
    adr_id: 7126c346-5816-49e9-b66e-702af7cb71d8
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Modules Generating Status Content Not Define

Scope: applies to `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STAT-001** MUST_NOT: Modules generating status content MUST NOT define localized copies of status domain strings or impact severity levels.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>