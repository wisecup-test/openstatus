---
version: 2
name: cross-cutting-path-module-adoption-modules-use-596f
description: "node:path Module Adoption: Modules Use Path Normalization Resolution Methods"
metadata:
  actual:
    adr_id: 596f9d2e-6ea5-4ff6-8857-4324d744cd43
    status: accepted
    candidate_group_id: 6b837a6c-0fec-4652-a0fa-6eb8b6a5ab1e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - next.config.ts
      - src/app/.well-known/agent-skills/index.json/**
      - src/content/**
      - src/lib/**
---

# node:path Module Adoption: Modules Use Path Normalization Resolution Methods

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/app/.well-known/agent-skills/index.json/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PATH-001** SHOULD: Modules SHOULD use path normalization and resolution methods provided by the module rather than assuming specific platform path separators.
- **R-PATH-002** MANDATORY: Ensure that all server-side and build-time path operations import the module exclusively through the protocol prefix.
- **R-PATH-003** MANDATORY: Confine path resolution logic to server-side utilities, route handlers, and build configuration to prevent client bundle leakage.
- **R-PATH-004** MANDATORY: Execute the lock-version grounding sequence (manifest discovery, build tool identification, lockfile inspection, official doc lookup, API confirmation, and version-sensitive re-run) before writing code that uses a versioned library.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify that all path module imports use the protocol-prefixed specifier.
# Execute the repository test suite to ensure that path resolution and content processing pass across execution environments.
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>