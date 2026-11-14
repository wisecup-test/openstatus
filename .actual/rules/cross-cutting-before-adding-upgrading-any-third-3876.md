---
version: 2
name: cross-cutting-before-adding-upgrading-any-third-3876
description: "Console API Logging: Before Adding Upgrading Any Third Party"
metadata:
  actual:
    adr_id: 38760f71-d762-4b76-99f8-4a8537fd6979
    status: accepted
    candidate_group_id: 0174bab9-9040-4ba3-9c11-4dcd869b8464
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/components/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Console API Logging: Before Adding Upgrading Any Third Party

Scope: applies to `apps/web/src/app/**`, `apps/web/src/components/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background routines, and utility functions within the project.

### Rules

- **R-LOG-001** MUST: Before adding or upgrading any third-party logging dependency, developers MUST inspect the repository dependency manifest and lock artifact to determine the exact resolved version.

### Verify

```bash
# Discover the project dependency manifest, test runner script, and static analysis/lint script, then execute them:
# 1. Inspect dependency manifest and lock file
# 2. Run test suite to verify log call execution
# 3. Run static analysis/lint script to check for prohibited patterns
```

**Accept when:**
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>