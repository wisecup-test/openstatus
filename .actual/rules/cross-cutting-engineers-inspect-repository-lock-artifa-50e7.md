---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-50e7
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Engineers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 50e7e41d-8674-4787-acaf-9a5b9631702f
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

# Adoption of Internal Content Utilities Module for Shared Content Operations: Engineers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONTENT-001** MUST: Engineers MUST inspect the repository lock artifact and verify the exact resolved version of any third-party content processing dependencies before modifying or introducing integrations in the content utilities module.

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