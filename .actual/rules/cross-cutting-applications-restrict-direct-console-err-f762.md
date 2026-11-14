---
version: 2
name: cross-cutting-applications-restrict-direct-console-err-f762
description: "Console API for Diagnostic and Operational Logging: Applications Restrict Direct Console Error Warning"
metadata:
  actual:
    adr_id: f7627bbe-ddee-45ac-978f-b7f66e5565fe
    status: accepted
    candidate_group_id: b26fa19d-bfc7-4c59-b7c9-13cbbd8e23ce
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
      - src/hooks/**
---

# Console API for Diagnostic and Operational Logging: Applications Restrict Direct Console Error Warning

Scope: applies to `packages/ui/scripts/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for build scripts and client-side hooks restricting direct console error and warning output.

### Rules

- **R-LOG-001** MUST: Applications MUST restrict direct console error and warning output to operational build scripts and fallback exception handlers.

### Verify

```bash
# Discover the project linter script from the project manifest and execute it to verify logging statements.
# Discover and run the project test runner to ensure diagnostic log statements do not cause unexpected test failures.
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>