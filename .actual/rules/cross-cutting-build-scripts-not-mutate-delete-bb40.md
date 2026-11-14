---
version: 2
name: cross-cutting-build-scripts-not-mutate-delete-bb40
description: "process.env Environment Variable Access: Build Scripts Not Mutate Delete Properties"
metadata:
  actual:
    adr_id: bb404c2e-1ca2-496f-9a4a-c9aa938d8636
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: Build Scripts Not Mutate Delete Properties

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build and transformation scripts executing in the workspace that access runtime environment variables.

### Rules

- **R-ENV-001** MUST_NOT: Build scripts MUST NOT mutate or delete properties on process.env during execution.

### Verify

```bash
# Discover the workspace script verification target in the package manifest and execute the designated test or lint suite.
npm test
```

**Accept when:**
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unhandled process.env dereferencing are blocked until fallback or validation logic is added.
</enforcement>