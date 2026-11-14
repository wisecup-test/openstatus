---
version: 2
name: cross-cutting-scripts-accessing-process-env-provide-cac5
description: "process.env Environment Variable Access: Scripts Accessing Process Env Provide Fallback"
metadata:
  actual:
    adr_id: cac5cb2e-fe11-4b53-a6bf-9cb2aa03799d
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: Scripts Accessing Process Env Provide Fallback

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build and transformation scripts executing in the workspace that access runtime environment variables.

### Rules

- **R-ENV-001** SHOULD: Scripts accessing process.env SHOULD provide fallback defaults or fail explicitly when required environment variables are undefined.

### Verify

```bash
# Discover and execute workspace script verification target
npm test || yarn test || pnpm test
# Inspect build script sources for process.env usage and fallback handling
```

**Accept when:**
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>