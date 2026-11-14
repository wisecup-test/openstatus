---
version: 2
name: cross-cutting-components-not-emit-sensitive-informatio-df59
description: "Console API for Diagnostic and Operational Logging: Components Not Emit Sensitive Information Unredacted"
metadata:
  actual:
    adr_id: df597dc3-9c7e-42d2-b738-7416d2863573
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

# Console API for Diagnostic and Operational Logging: Components Not Emit Sensitive Information Unredacted

Scope: applies to `packages/ui/scripts/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for build automation scripts reporting execution prerequisites or configuration failures and client-side hooks capturing non-fatal runtime interaction errors and fallback conditions.

### Rules

- **R-LOG-001** MUST_NOT: Components MUST NOT emit sensitive information or unredacted user data through direct console methods.

### Verify

```bash
# Discover and run the repository linter script from the project manifest
# Discover and run the project test runner to verify diagnostic log statements
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in continuous integration pipelines and peer code review.
</enforcement>