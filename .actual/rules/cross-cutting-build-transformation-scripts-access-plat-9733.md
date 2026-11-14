---
version: 2
name: cross-cutting-build-transformation-scripts-access-plat-9733
description: "Direct Process Environment Variable Access via Process Module: Build Transformation Scripts Access Platform Runtime"
metadata:
  actual:
    adr_id: 973388b6-c46c-40e9-b156-5d74dd9bb028
    status: accepted
    candidate_group_id: 641a54a0-56d3-41a2-a284-dcbd11067dc6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Direct Process Environment Variable Access via Process Module: Build Transformation Scripts Access Platform Runtime

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build automation scripts, transformation workflows, and local development execution environments requiring platform deployment context.

### Rules

- **R-ENV-001** MUST: Build and transformation scripts MUST access platform runtime configuration values directly through the global process environment object rather than introducing external configuration management dependencies.

### Verify

```bash
# Discover and run repository verification scripts to confirm environment configuration parsing complies with project standards.
# Execute repository linting suites to ensure process environment access remains restricted to authorized utility scripts.
find . -type f -name "*verify*" -o -name "*lint*" | xargs -I {} echo "Found verification script: {}"
```

**Accept when:**
- Repository verification scripts pass without reporting unauthorized process environment access.
- Utility scripts successfully parse required platform environment variables and exit cleanly.

<enforcement>
Claude Code MUST NOT skip or defer verification. All modifications to build automation routines must be verified via static analysis checks and peer code review, and violations introducing direct environment reads in production application modules will be blocked.
</enforcement>