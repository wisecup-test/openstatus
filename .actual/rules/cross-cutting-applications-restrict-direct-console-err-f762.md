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

These rules are ALWAYS ACTIVE for build automation scripts reporting execution prerequisites, configuration failures, and client-side hooks capturing non-fatal runtime interaction errors and fallback conditions.

### Rules

- **R-CON-001** MUST: Applications MUST restrict direct console error and warning output to operational build scripts and fallback exception handlers.

### Verify

```bash
# Discover the repository linter script from the project manifest and execute it
LINTER=$(npm pkg get scripts.lint 2>/dev/null | tr -d '"')
if [ -n "$LINTER" ] && [ "$LINTER" != "null" ]; then
  npm run lint
else
  npx eslint .
fi

# Discover and run the project test runner
TEST_RUNNER=$(npm pkg get scripts.test 2>/dev/null | tr -d '"')
if [ -n "$TEST_RUNNER" ] && [ "$TEST_RUNNER" != "null" ]; then
  npm test
else
  echo "No test script defined"
fi
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>