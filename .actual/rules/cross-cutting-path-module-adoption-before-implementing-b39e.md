---
version: 2
name: cross-cutting-path-module-adoption-before-implementing-b39e
description: "node:path Module Adoption: Before Implementing Updating Path Manipulation Logic"
metadata:
  actual:
    adr_id: b39e4869-8ef4-42f7-b1a0-c9ed773d57d1
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

# node:path Module Adoption: Before Implementing Updating Path Manipulation Logic

Scope: applies to `apps/web/next.config.ts`, `apps/web/src/app/.well-known/agent-skills/index.json/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for server-side utilities, route handlers, build configuration logic, and component rendering logic executing strictly within server runtime boundaries.

### Rules

- **R-PATH-001** MUST: Before implementing or updating path manipulation logic, the consumer MUST inspect the repository lock artifact to verify the runtime platform specification and resolved compatibility baseline.
- **R-PATH-002** MUST: Ensure that all server-side and build-time path operations import the node:path module exclusively through the protocol prefix (`node:path`).
- **R-PATH-003** MUST: Confine path resolution logic to server-side utilities, route handlers, and build configuration to prevent client bundle leakage.
- **R-PATH-004** MUST: Mandate the use of `join` and `resolve` methods from the module instead of manual separator string formatting.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify protocol-prefixed path module imports
npm run lint || npx eslint .
# Execute the repository test suite
npm test
```

**Accept when:**
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations must be updated to the protocol-prefixed module specifier before merge approval.
</enforcement>