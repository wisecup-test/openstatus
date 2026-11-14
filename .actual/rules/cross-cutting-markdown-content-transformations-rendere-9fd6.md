---
version: 2
name: cross-cutting-markdown-content-transformations-rendere-9fd6
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Markdown Content Transformations Rendered Within User"
metadata:
  actual:
    adr_id: 9fd6caf9-ba5d-4b36-ad5f-45204802937a
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Markdown Content Transformations Rendered Within User

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MD-001** MUST: All markdown content transformations rendered within user interface message components MUST process text through a unified pipeline chaining remark-parse, remark-rehype, and rehype-react into React virtual DOM elements.

### Verify

```bash
# Discover and run static analysis and linter scripts defined in the project dependency manifest
# Discover and execute the component test suite defined in the project manifest
# Discover and run the project type checker
```

**Accept when:**
- All message rendering components parse markdown through the unified pipeline without invoking raw HTML injection properties.
- Static type analysis and component tests pass without errors across all markdown display modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>