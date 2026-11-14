---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-b49e
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Engineers Discover Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: b49eb861-cb35-4f1b-9daf-cdf4b1b05456
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Engineers Discover Repository Dependency Manifest Authoritative

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.

### Rules

- **R-NODE-001** MUST: Engineers MUST discover the repository dependency manifest and authoritative lock artifact to verify runtime and engine version constraints before implementing or updating automation scripts.
- **R-NODE-002** MUST: Declare module imports using the explicit protocol specifier prefix to disambiguate built-in runtime modules from external dependencies when authoring workspace automation routines.
- **R-NODE-003** MUST: Ensure build and transformation routines read configuration and schema files synchronously during initial execution and fail early with diagnostic messaging when prerequisites are missing.

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