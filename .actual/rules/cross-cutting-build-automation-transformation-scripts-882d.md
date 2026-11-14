---
version: 2
name: cross-cutting-build-automation-transformation-scripts-882d
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Build Automation Transformation Scripts Requiring Runtime"
metadata:
  actual:
    adr_id: 882d553f-ccc1-4be6-90ee-a076ba93dd11
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Build Automation Transformation Scripts Requiring Runtime

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOD-001** MUST: All build automation and transformation scripts requiring runtime filesystem access, path resolution, or URL handling MUST import Node.js core modules exclusively via the explicit node protocol namespace.

### Verify

```bash
# Discover the workspace validation task from repository configuration and run the script linter
# Search the script directory to confirm all core module imports use the explicit protocol prefix
```

**Accept when:**
- All automation script files importing core runtime capabilities use the explicit protocol namespace without bare specifiers.
- The discovered repository script validation and linting suite completes with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>