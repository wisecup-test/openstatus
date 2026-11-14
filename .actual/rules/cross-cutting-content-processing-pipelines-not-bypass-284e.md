---
version: 2
name: cross-cutting-content-processing-pipelines-not-bypass-284e
description: "unified AST Middleware Pipeline for React Content Transformation: Content Processing Pipelines Not Bypass Unified"
metadata:
  actual:
    adr_id: 284e7865-fcca-452c-a208-8b68eb03fc1b
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Content Processing Pipelines Not Bypass Unified

Scope: applies to `apps/status-page/src/components/content/**`.

These rules are ALWAYS ACTIVE for components rendering markdown content into React element trees.

### Rules

- **R-AST-001** MUST_NOT: Content processing pipelines MUST NOT bypass unified middleware parsing to inject unsanitized raw HTML strings into the component tree.

### Verify

```bash
# Discover and run static analysis, linting, and automated test suite scripts from the repository configuration.
npm run lint || yarn lint || pnpm lint
npm test || yarn test || pnpm test
```

**Accept when:**
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>