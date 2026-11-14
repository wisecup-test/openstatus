---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-d25c
description: "Adoption of Shared checker/utils Internal Module: Engineers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: d25c8c23-6eeb-4c7d-89d1-e5440a21815a
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Engineers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all features, presentation components, and endpoints that process, format, or present checker domain data.

### Rules

- **R-CHK-001** MUST: Engineers MUST inspect the repository lock artifact to verify resolved dependency versions before integrating third-party packages or modifying module contracts.

### Verify

```bash
# Discover and run the project test runner across all test suites covering shared domain utilities
# Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules
```

**Accept when:**
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code review.
</enforcement>