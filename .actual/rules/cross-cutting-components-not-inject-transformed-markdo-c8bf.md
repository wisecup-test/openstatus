---
version: 2
name: cross-cutting-components-not-inject-transformed-markdo-c8bf
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Components Not Inject Transformed Markdown Strings"
metadata:
  actual:
    adr_id: c8bf743b-82d6-4c16-b57f-62cfe1c45c09
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Components Not Inject Transformed Markdown Strings

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-side user interface components rendering markdown strings from chat or message streams and dashboard content displays requiring custom component mapping for links and formatted code.

### Rules

- **R-MARKDOWN-001** MUST_NOT: Components MUST_NOT inject transformed markdown strings into the document object model using raw HTML injection properties.
- **R-MARKDOWN-002** MANDATORY: Memoize unified pipeline instances or their compiled processor output within rendering hooks to prevent re-instantiating the parser and compiler chain across component re-renders.
- **R-MARKDOWN-003** MANDATORY: Provide explicit type annotations for component replacement dictionaries passed to rehype-react to ensure safe property forwarding to replacement elements.
- **R-MARKDOWN-004** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library (find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official docs for that exact version, confirm APIs exist, re-run per dependency at point of use).

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
Claude Code MUST NOT skip or defer verification. Automated code review and lint checks enforce that UI components do not use direct raw HTML injection.
</enforcement>