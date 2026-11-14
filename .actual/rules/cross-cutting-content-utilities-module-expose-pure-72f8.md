---
version: 2
name: cross-cutting-content-utilities-module-expose-pure-72f8
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Content Utilities Module Expose Pure Deterministic"
metadata:
  actual:
    adr_id: 72f8c728-0fdf-426e-ba63-daa2de5ca038
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

# Adoption of Internal Content Utilities Module for Shared Content Operations: Content Utilities Module Expose Pure Deterministic

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD: The content utilities module SHOULD expose pure, deterministic utility functions with minimal side effects to remain safe for execution across both static generation and runtime rendering contexts.

### Verify

```bash
# Discover and execute the repository type-checking script from the root configuration
# Discover and execute the automated test suite targeting content utility functions
# Discover and execute the static analysis linter script
```

**Accept when:**
- All content utility functions pass automated unit tests without failures.
- Type checking completes with zero diagnostic errors across all consumer modules.
- No circular dependencies or duplicate content parsing implementations are detected by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>