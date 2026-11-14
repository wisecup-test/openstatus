---
version: 2
name: cross-cutting-custom-element-styling-applied-passing-3333
description: "unified AST Middleware Pipeline for React Content Transformation: Custom Element Styling Applied Passing Component"
metadata:
  actual:
    adr_id: 33330bde-7eac-4386-bee6-d9f87b844613
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Custom Element Styling Applied Passing Component

Scope: applies to `apps/status-page/src/components/content/**`.

These rules are ALWAYS ACTIVE for components rendering markdown content into React element trees.

### Rules

- **R-AST-001** SHOULD: Custom element styling SHOULD be applied by passing component mapping functions to the rehypeReact plugin configuration rather than post-processing rendered trees.

### Verify

```bash
# Discover and run project static analysis / linting and test suite scripts from the repository manifest
```

**Accept when:**
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline checks and peer code reviews enforce these requirements.
</enforcement>