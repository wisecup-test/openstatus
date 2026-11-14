---
version: 2
name: cross-cutting-middleware-pipelines-rendering-tabular-e-b9ca
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Middleware Pipelines Rendering Tabular Extended Markdown"
metadata:
  actual:
    adr_id: b9cae650-72d1-4e9d-9289-f91b5c772cdb
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Middleware Pipelines Rendering Tabular Extended Markdown

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-side user interface components rendering markdown strings from chat, message streams, or dashboard content displays.

### Rules

- **R-UP-001** MAY: Middleware pipelines rendering tabular or extended markdown content MAY include remark-gfm in the processing chain prior to the HTML AST bridge.

### Verify

```bash
# Discover and run the static analysis and linter scripts defined in the project dependency manifest
# Discover and execute the component test suite defined in the project manifest
# Discover and run the project type checker
```

**Accept when:**
- All message rendering components parse markdown through the unified pipeline without invoking raw HTML injection properties.
- Static type analysis and component tests pass without errors across all markdown display modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>