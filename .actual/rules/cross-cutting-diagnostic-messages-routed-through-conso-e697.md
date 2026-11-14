---
version: 2
name: cross-cutting-diagnostic-messages-routed-through-conso-e697
description: "Console API for Diagnostic and Operational Logging: Diagnostic Messages Routed Through Console Methods"
metadata:
  actual:
    adr_id: e697b91d-7a69-45b9-b586-a674a4d699f9
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

# Console API for Diagnostic and Operational Logging: Diagnostic Messages Routed Through Console Methods

Scope: applies to `packages/ui/scripts/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for build automation scripts reporting execution prerequisites or configuration failures, and client-side hooks capturing non-fatal runtime interaction errors and fallback conditions.

### Rules

- **R-LOG-001** SHOULD: Diagnostic messages routed through console methods SHOULD provide actionable remediation guidance alongside failure notifications.

### Verify

```bash
# Discover and run the project linter and test runner
LINTER=$(npm pkg get scripts.lint --json 2>/dev/null | tr -d '"')
if [ -n "$LINTER" ] && [ "$LINTER" != "null" ]; then
  npm run lint
fi

TEST_RUNNER=$(npm pkg get scripts.test --json 2>/dev/null | tr -d '"')
if [ -n "$TEST_RUNNER" ] && [ "$TEST_RUNNER" != "null" ]; then
  npm test
fi
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>