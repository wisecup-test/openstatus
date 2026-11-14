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

- **R-PATH-001** SHOULD: Modules SHOULD use path normalization and resolution methods provided by the `node:path` module rather than assuming specific platform path separators.
- **R-PATH-002** MANDATORY: Server-side and build-time path operations MUST import the `node:path` module exclusively through the protocol-prefixed specifier.
- **R-PATH-003** MANDATORY: Confine path resolution logic strictly to server-side utilities, route handlers, and build configuration to prevent client bundle leakage.
- **R-PATH-004** MANDATORY: Developers MUST follow LOCK-VERSION GROUNDING before writing code that uses a versioned library by checking the dependency manifest, identifying the build tool, inspecting the lock or resolution artifact for the exact resolved version, looking up official documentation for that version, and confirming every API call exists in that version.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify path module imports
# (Command must be derived from the project repository configuration)
if [ -f "package.json" ]; then
  npm run lint || npx eslint .
fi

# Execute the repository test suite
if [ -f "package.json" ]; then
  npm test
fi
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing bare un-prefixed module imports or ad-hoc path concatenations fail automated verification and must be updated to the protocol-prefixed module specifier before merge approval.
</enforcement>