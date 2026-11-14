---
version: 2
name: cross-cutting-pipeline-configurations-pass-custom-elem-6efa
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Pipeline Configurations Pass Custom Element Mappings"
metadata:
  actual:
    adr_id: 6efaaa2b-7815-4b71-8e58-eea396b6f3ca
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Pipeline Configurations Pass Custom Element Mappings

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-side user interface components rendering markdown strings from chat, message streams, or dashboard content displays.

### Rules

- **R-PIPE-001** SHOULD: Pipeline configurations SHOULD pass custom element mappings into rehype-react to standardize anchor link routing and code block styling across chat and message displays.
- **R-PIPE-002** MANDATORY: Consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository (discovery policy).
- **R-PIPE-003** MANDATORY: Consumer MUST execute lock-version grounding before writing code that uses a versioned library (manifest -> build tool -> lock/resolution artifact -> docs/changelog -> API confirmation).
- **R-PIPE-004** MANDATORY: Memoize unified pipeline instances or their compiled processor output within rendering hooks to prevent re-instantiating the parser and compiler chain across component re-renders.
- **R-PIPE-005** MANDATORY: Provide explicit type annotations for component replacement dictionaries passed to rehype-react to ensure safe property forwarding to replacement elements.

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
Claude Code MUST NOT skip or defer verification. Automated code review, unit/integration tests, and static analysis enforce compliance, and violations block pull requests.
</enforcement>