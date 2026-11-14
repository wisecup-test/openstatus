---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-0758
description: "unified AST Middleware Pipeline for React Content Transformation: Engineers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 0758cd19-1349-48bb-9d23-fd6bd8c56591
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Engineers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/status-page/src/components/content/**`.

These rules are ALWAYS ACTIVE for components rendering markdown content into React element trees.

### Rules

- **R-AST-001** MUST: Engineers MUST inspect the repository lock artifact and verify the exact resolved versions of unified, remarkParse, remarkRehype, and rehypeReact before implementing content pipeline extensions.

### Verify

```bash
# Discover and run the project static analysis, linting, and automated test suite scripts from the repository dependency manifest and configuration.
npm run lint
npm test
```

**Accept when:**
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>