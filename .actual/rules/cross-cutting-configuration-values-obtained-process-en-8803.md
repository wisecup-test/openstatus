---
version: 2
name: cross-cutting-configuration-values-obtained-process-en-8803
description: "process.env Environment Variable Access: Configuration Values Obtained Process Env That"
metadata:
  actual:
    adr_id: 88033dd7-0709-4a1d-b438-46a5f4e7a32f
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: Configuration Values Obtained Process Env That

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build and transformation scripts executing in the workspace that access runtime environment variables.

### Rules

- **R-ENV-001** SHOULD: Configuration values obtained from process.env that represent external endpoints or URLs SHOULD be validated for expected structure prior to consumption.

### Verify

```bash
# Discover and execute the workspace script verification target in the package manifest
npm test
npm run lint
```

**Accept when:**
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>