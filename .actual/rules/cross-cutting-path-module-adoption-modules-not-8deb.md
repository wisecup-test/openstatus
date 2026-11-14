---
version: 2
name: cross-cutting-path-module-adoption-modules-not-8deb
description: "node:path Module Adoption: Modules Not Use Bare Prefixed Specifiers"
metadata:
  actual:
    adr_id: 8deb6074-1ba6-4e4f-b1d7-0814715a9280
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

# node:path Module Adoption: Modules Not Use Bare Prefixed Specifiers

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/app/.well-known/agent-skills/index.json/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for server-side utilities, route handlers, build configuration logic, and component rendering logic executing strictly within server runtime boundaries.

### Rules

- **R-PATH-001** MUST_NOT: Modules MUST NOT use bare un-prefixed specifiers or manual string concatenation to construct or manipulate filesystem paths.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Example discovery step:
# npm run lint || yarn lint || pnpm lint
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>