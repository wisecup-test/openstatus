---
version: 2
name: cross-cutting-scripts-encountering-missing-generation-6e23
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Scripts Encountering Missing Generation Artifacts Required"
metadata:
  actual:
    adr_id: 6e23aa59-5e0f-4d84-8c9a-e78842852732
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Scripts Encountering Missing Generation Artifacts Required

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for all build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.

### Rules

- **R-NS-001** MUST: Scripts encountering missing generation artifacts or required paths MUST emit actionable error diagnostics to the error stream and terminate with a non-zero exit code.

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