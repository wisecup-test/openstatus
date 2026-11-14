---
version: 2
name: cross-cutting-path-module-adoption-modules-designed-826e
description: "node:path Module Adoption: Modules Designed Client Side Browser Execution"
metadata:
  actual:
    adr_id: 826e8d7d-4766-44a0-b4fb-a52357cb58f5
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

# node:path Module Adoption: Modules Designed Client Side Browser Execution

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/app/.well-known/agent-skills/index.json/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for modules designed for client-side browser execution.

### Rules

- **R-PATH-001** MUST_NOT: Modules designed for client-side browser execution MUST NOT import the module.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify that all path module imports use the protocol-prefixed specifier
# Execute the repository test suite to ensure that path resolution and content processing pass across execution environments
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>