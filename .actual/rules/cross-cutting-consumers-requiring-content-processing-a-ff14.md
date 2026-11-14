---
version: 2
name: cross-cutting-consumers-requiring-content-processing-a-ff14
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Consumers Requiring Content Processing Across Data"
metadata:
  actual:
    adr_id: ff1437a9-9443-48dc-af7e-07c520d91f16
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

# Adoption of Internal Content Utilities Module for Shared Content Operations: Consumers Requiring Content Processing Across Data

Scope: applies to `apps/web/src/app/**`, `apps/web/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Consumers requiring content processing across data access layers, sitemap generation routines, and error views MUST import content operations exclusively from the centralized content utilities module.

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