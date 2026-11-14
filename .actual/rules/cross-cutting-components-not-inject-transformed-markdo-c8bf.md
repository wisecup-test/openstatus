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

These rules are ALWAYS ACTIVE for client-side user interface components rendering markdown strings from chat, message streams, or dashboard content displays requiring custom component mapping.

### Rules

- **R-MD-001** MUST_NOT: Components MUST_NOT inject transformed markdown strings into the document object model using raw HTML injection properties.
- **R-MD-002** MANDATORY: Memoize unified pipeline instances or their compiled processor output within rendering hooks to prevent re-instantiating the parser and compiler chain across component re-renders.
- **R-MD-003** MANDATORY: Provide explicit type annotations for component replacement dictionaries passed to rehype-react to ensure safe property forwarding to replacement elements.
- **R-MD-004** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library (find manifest, identify build tool, inspect lock/resolution artifact, look up official docs for that exact version, confirm APIs exist, re-run per dependency at point of use).

### Verify

```bash
# Discover and run the static analysis and linter scripts defined in the project dependency manifest
# Discover and execute the component test suite defined in the project manifest
# Discover and run the project type checker to verify unified processor and component mapping type signatures
```

**Accept when:**
- All message rendering components parse markdown through the unified pipeline without invoking raw HTML injection properties.
- Static type analysis and component tests pass without errors across all markdown display modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated code review, lint checks, unit tests, and integration tests validating markdown output maps to expected React component hierarchies. Violations block pull requests; exceptions require documented architecture review and technical lead security sign-off.
</enforcement>