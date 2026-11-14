---
version: 2
name: cross-cutting-application-modules-business-logic-not-49eb
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Application Modules Business Logic Not Directly"
metadata:
  actual:
    adr_id: 49eb004f-587e-4fcb-9aed-f77b0cc3d4c5
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Application Modules Business Logic Not Directly

Scope: applies to `packages/notifications/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Application modules and business logic MUST NOT directly read raw process environment variables outside the dedicated environment configuration module.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
typecheck_cmd=$(npm pkg get scripts.typecheck --json 2>/dev/null | tr -d '"' || echo "")
if [ -n "$typecheck_cmd" ] && [ "$typecheck_cmd" != "null" ]; then
  npm run typecheck
else
  pnpm typecheck || yarn typecheck || bun run typecheck
fi

# Discover and run the project linter to confirm direct process.env access is restricted
lint_cmd=$(npm pkg get scripts.lint --json 2>/dev/null | tr -d '"' || echo "")
if [ -n "$lint_cmd" ] && [ "$lint_cmd" != "null" ]; then
  npm run lint
else
  pnpm lint || yarn lint || bun run lint
fi

# Discover and run the project test script with sample environment values
test_cmd=$(npm pkg get scripts.test --json 2>/dev/null | tr -d '"' || echo "")
if [ -n "$test_cmd" ] && [ "$test_cmd" != "null" ]; then
  npm test
else
  pnpm test || yarn test || bun run test
fi
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>