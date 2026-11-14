---
version: 2
name: cross-cutting-application-use-unified-processing-pipel-6742
description: "unified AST Middleware Pipeline for React Content Transformation: Application Use Unified Processing Pipeline When"
metadata:
  actual:
    adr_id: 67424298-9001-45b4-9530-4fe43512c7a6
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Application Use Unified Processing Pipeline When

Scope: applies to `apps/status-page/src/components/content/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UNIFIED-001** MUST: The application MUST use the unified processing pipeline when converting markdown content into React component structures.

### Verify

```bash
# Discover and run static analysis, linting, and automated tests from the repository configuration.
# (Exact commands derived from the repository dependency manifest)
```

**Accept when:**
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling blocks pull requests introducing raw HTML injection or unapproved markdown parsers.
</enforcement>