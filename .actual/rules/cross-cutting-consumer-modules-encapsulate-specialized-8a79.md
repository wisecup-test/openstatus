---
version: 2
name: cross-cutting-consumer-modules-encapsulate-specialized-8a79
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Consumer Modules Encapsulate Specialized Presentation Formatting"
metadata:
  actual:
    adr_id: 8a7940a0-76b8-4953-ab2f-2780601b4b63
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

# Adoption of Internal Content Utilities Module for Shared Content Operations: Consumer Modules Encapsulate Specialized Presentation Formatting

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONTENT-001** MAY: Consumer modules MAY encapsulate specialized presentation formatting locally if the logic is unique to a single presentation component and does not operate on shared content schemas.

### Verify

```bash
# Discover and execute type-checking script from root configuration
# Discover and execute automated test suite targeting content utility functions
# Discover and execute static analysis linter script
```

**Accept when:**
- All content utility functions pass automated unit tests without failures.
- Type checking completes with zero diagnostic errors across all consumer modules.
- No circular dependencies or duplicate content parsing implementations are detected by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing type verification and unit tests, and peer code review.
</enforcement>