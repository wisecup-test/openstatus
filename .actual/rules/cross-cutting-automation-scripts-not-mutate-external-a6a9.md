---
version: 2
name: cross-cutting-automation-scripts-not-mutate-external-a6a9
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Automation Scripts Not Mutate External Environment"
metadata:
  actual:
    adr_id: a6a9d551-5a38-4ae3-9e3d-dbed4daf126e
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Automation Scripts Not Mutate External Environment

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.

### Rules

- **R-ENV-001** SHOULD_NOT: Automation scripts SHOULD NOT mutate external environment variables and MUST access deployment runtime flags strictly through the standard runtime environment interface.

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