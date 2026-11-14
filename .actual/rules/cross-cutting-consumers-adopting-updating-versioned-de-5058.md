---
version: 2
name: cross-cutting-consumers-adopting-updating-versioned-de-5058
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Consumers Adopting Updating Versioned Dependencies Within"
metadata:
  actual:
    adr_id: 5058c0ab-83e0-4599-8674-8c339b753677
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Consumers Adopting Updating Versioned Dependencies Within

Scope: applies to `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STV-001** MUST: Consumers adopting or updating versioned dependencies within the status pipeline MUST discover the repository lock artifact and verify the exact resolved dependency version prior to implementation.

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