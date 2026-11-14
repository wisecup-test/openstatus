---
version: 2
name: cross-cutting-before-implementing-updating-unified-pip-39f4
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Before Implementing Updating Unified Pipeline Plugins"
metadata:
  actual:
    adr_id: 39f47f7d-6fca-41c1-a56c-e69f5a19080e
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Before Implementing Updating Unified Pipeline Plugins

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UNIFIED-001** MUST: Before implementing or updating unified pipeline plugins, the consumer MUST inspect the repository lock artifact to verify the exact resolved version of each dependency and confirm API compatibility against official documentation.
- **R-UNIFIED-002** MUST: Memoize unified pipeline instances or their compiled processor output within rendering hooks to prevent re-instantiating the parser and compiler chain across component re-renders.
- **R-UNIFIED-003** MUST: Provide explicit type annotations for component replacement dictionaries passed to rehype-react to ensure safe property forwarding to replacement elements.

### Verify

```bash
# Discover and run the static analysis and linter scripts defined in the project dependency manifest to enforce forbidden raw HTML properties.
# Discover and execute the component test suite defined in the project manifest to validate markdown rendering and link handling.
# Discover and run the project type checker to verify unified processor and component mapping type signatures.
```

**Accept when:**
- All message rendering components parse markdown through the unified pipeline without invoking raw HTML injection properties.
- Static type analysis and component tests pass without errors across all markdown display modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All markdown rendering components must safely process AST structures via unified and rehype-react without bypassing the pipeline or utilizing unverified plugin versions.
</enforcement>