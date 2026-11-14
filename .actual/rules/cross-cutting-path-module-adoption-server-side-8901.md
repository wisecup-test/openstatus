---
version: 2
name: cross-cutting-path-module-adoption-server-side-8901
description: "node:path Module Adoption: Server Side Build Time Modules Requiring"
metadata:
  actual:
    adr_id: 89014592-0afa-46d2-8063-2d8d462e1ba8
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

# node:path Module Adoption: Server Side Build Time Modules Requiring

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/app/.well-known/agent-skills/index.json/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all server-side utilities, route handlers, build configuration logic, and component rendering logic executing strictly within server runtime boundaries.

### Rules

- **R-PATH-001** MUST: All server-side and build-time modules requiring path resolution, joining, or normalization MUST import the module via the protocol-prefixed specifier `node:path`.

### Verify

```bash
# Discover and run project static analysis and linting scripts to verify path module imports use the protocol-prefixed specifier
if npm run | grep -q "lint"; then
  npm run lint
elif npx eslint --version > /dev/null 2>&1; then
  npx eslint .
fi

# Execute the repository test suite
if npm test --if-present; then
  npm test
fi
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier (`node:path`).
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>