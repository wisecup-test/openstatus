---
version: 2
name: cross-cutting-automation-scripts-not-import-core-ec4d
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Automation Scripts Not Import Core Runtime"
metadata:
  actual:
    adr_id: ec4d28f4-90f8-4292-9a60-aeeb0945012b
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Automation Scripts Not Import Core Runtime

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.

### Rules

- **R-AUTO-001** MUST_NOT: Automation scripts MUST NOT import core runtime capabilities using bare module specifiers lacking the protocol prefix.

### Verify

```bash
# Discover the workspace validation task from repository configuration and run the script linter
# Search the script directory to confirm all core module imports use the explicit protocol prefix
```

**Accept when:**
- All automation script files importing core runtime capabilities use the explicit protocol namespace without bare specifiers.
- The discovered repository script validation and linting suite completes with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository script linting rules and peer code reviews.
</enforcement>