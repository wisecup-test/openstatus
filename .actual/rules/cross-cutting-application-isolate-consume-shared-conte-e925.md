---
version: 2
name: cross-cutting-application-isolate-consume-shared-conte-e925
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Application Isolate Consume Shared Content Transformation"
metadata:
  actual:
    adr_id: e92549f7-5aa3-4ba3-b7ed-95670fd3349a
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

# Adoption of Internal Content Utilities Module for Shared Content Operations: Application Isolate Consume Shared Content Transformation

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONTENT-001** MUST: The application MUST isolate and consume all shared content transformation, normalization, and parsing operations through the internal content utilities module rather than reimplementing ad-hoc content parsing logic in consumer components.

### Verify

```bash
# Discover and execute the repository type-checking script from the root configuration to verify module import resolution and interface contracts.
# Discover and execute the automated test suite targeting content utility functions to validate operational correctness.
# Discover and execute the static analysis linter script to ensure compliance with import path conventions.
```

**Accept when:**
- All content utility functions pass automated unit tests without failures.
- Type checking completes with zero diagnostic errors across all consumer modules.
- No circular dependencies or duplicate content parsing implementations are detected by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>