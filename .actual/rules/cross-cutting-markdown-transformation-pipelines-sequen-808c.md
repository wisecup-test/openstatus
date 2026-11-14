---
version: 2
name: cross-cutting-markdown-transformation-pipelines-sequen-808c
description: "unified AST Middleware Pipeline for React Content Transformation: Markdown Transformation Pipelines Sequence Remarkparse Generate"
metadata:
  actual:
    adr_id: 808c707e-b451-40f2-880b-fb73c0545a8e
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Markdown Transformation Pipelines Sequence Remarkparse Generate

Scope: applies to `apps/status-page/src/components/content/**`.

These rules are ALWAYS ACTIVE for components rendering markdown content into React element trees.

### Rules

- **R-AST-001** MUST: Markdown transformation pipelines MUST sequence remarkParse to generate markdown AST, remarkRehype to bridge to HTML AST, and rehypeReact to emit React elements.

### Verify

```bash
# Discover and run the project static analysis, linting, and automated test suite scripts from the repository dependency manifest.
```

**Accept when:**
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>