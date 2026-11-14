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

These rules are ALWAYS ACTIVE for build automation scripts reporting prerequisites/configuration failures, and client-side hooks capturing non-fatal runtime interaction errors and fallback conditions.

### Rules

- **R-CON-001** SHOULD: Diagnostic messages routed through console methods SHOULD provide actionable remediation guidance alongside failure notifications.

### Verify

```bash
# Discover the repository linter script from the project manifest and execute it to verify logging statements.
LINTER=$(node -p "try { const pkg = require('./package.json'); Object.keys(pkg.scripts || {}).find(s => s.includes('lint')) || 'npm run lint' } catch(e) { 'npm run lint' }")
eval $Linter

# Discover and run the project test runner to ensure diagnostic log statements do not cause unexpected test failures.
TEST_RUNNER=$(node -p "try { const pkg = require('./package.json'); Object.keys(pkg.scripts || {}).find(s => s.includes('test')) || 'npm test' } catch(e) { 'npm test' }")
eval $TEST_RUNNER
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>