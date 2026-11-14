---
version: 2
name: cross-cutting-status-aggregation-workflows-maintain-co-19bd
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Status Aggregation Workflows Maintain Component Tracking"
metadata:
  actual:
    adr_id: 19bd16c1-3eb7-447b-8d0d-ec7e99dc59a8
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Status Aggregation Workflows Maintain Component Tracking

Scope: applies to `apps/status-page/src/content/markdown/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** SHOULD: Status aggregation workflows SHOULD maintain component tracking mappings to correlate component identifiers with impact states resolved through the status-vocab module.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline running static analysis and type checks, and peer code review.
</enforcement>