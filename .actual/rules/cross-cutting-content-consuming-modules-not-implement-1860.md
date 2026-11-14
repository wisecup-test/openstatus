---
version: 2
name: cross-cutting-content-consuming-modules-not-implement-1860
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Content Consuming Modules Not Implement Duplicate"
metadata:
  actual:
    adr_id: 1860590a-5ea7-4c07-8427-44b5830523db
    status: accepted
    candidate_group_id: 93b03f54-7c9b-4fbb-b40e-541b4d07014e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/data/**
---

# Adoption of Internal Content Utilities Module for Shared Content Operations: Content Consuming Modules Not Implement Duplicate

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONT-001** MUST_NOT: Content-consuming modules MUST NOT implement duplicate utility functions for content metadata extraction or slug resolution outside the internal content utilities module.

### Verify

```bash
# Discover and execute the repository type-checking script from root configuration
# Discover and execute the automated test suite targeting content utility functions
# Discover and execute the static analysis linter script
```

**Accept when:**
- All content utility functions pass automated unit tests without failures.
- Type checking completes with zero diagnostic errors across all consumer modules.
- No circular dependencies or duplicate content parsing implementations are detected by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated CI checks, peer code review, and architectural exception processes.
</enforcement>