---
version: 2
name: cross-cutting-scripts-fallback-default-local-values-5c6b
description: "Direct Process Environment Variable Access via Process Module: Scripts Fallback Default Local Values When"
metadata:
  actual:
    adr_id: 5c6b0be9-bf4b-4bd3-8742-81ce62deb28e
    status: accepted
    candidate_group_id: 641a54a0-56d3-41a2-a284-dcbd11067dc6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Direct Process Environment Variable Access via Process Module: Scripts Fallback Default Local Values When

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MAY: Scripts MAY fallback to default local values when deployment platform environment variables are absent from the runtime environment.

### Verify

```bash
# Discover and run the repository verification scripts to confirm environment configuration parsing complies with project standards.
# Execute repository linting suites to ensure process environment access remains restricted to authorized utility scripts.
```

**Accept when:**
- Repository verification scripts pass without reporting unauthorized process environment access.
- Utility scripts successfully parse required platform environment variables and exit cleanly.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>