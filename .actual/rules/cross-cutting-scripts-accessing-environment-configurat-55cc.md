---
version: 2
name: cross-cutting-scripts-accessing-environment-configurat-55cc
description: "process.env Environment Variable Access: Scripts Accessing Environment Configurations Through Process"
metadata:
  actual:
    adr_id: 55ccb2b2-4f4c-40cb-ad7c-bbc7de46558f
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: Scripts Accessing Environment Configurations Through Process

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build and transformation scripts executing in the workspace that access runtime environment variables.

### Rules

- **R-ENV-001** MUST: Scripts accessing environment configurations through process.env MUST restrict reads to documented configuration keys required for execution.

### Verify

```bash
# Discover workspace script verification target in the package manifest and execute the designated test or lint suite
# Inspect build script sources to verify that all process.env accesses include existence checks or fallback handling
```

**Accept when:**
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>