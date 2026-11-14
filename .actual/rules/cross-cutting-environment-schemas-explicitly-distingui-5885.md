---
version: 2
name: cross-cutting-environment-schemas-explicitly-distingui-5885
description: "@t3-oss/env-core Environment Variable Schema Validation: Environment Schemas Explicitly Distinguish Between Server"
metadata:
  actual:
    adr_id: 5885efe1-3bb3-46aa-a147-fd1127b94399
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Environment Schemas Explicitly Distinguish Between Server

Scope: applies to `packages/analytics/**`.

These rules are ALWAYS ACTIVE for modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

### Rules

- **R-ENV-001** MUST: Environment schemas MUST explicitly distinguish between server-only secret configurations and client-accessible public variables.

### Verify

```bash
# Discover the project test script from the dependency manifest and execute the test runner to verify environment schema parsing.
npm test

# Discover and run the project type checking script to ensure exported environment configurations satisfy static type signatures.
npm run typecheck

# Discover and execute the project linting and boundary verification scripts to confirm no direct accesses to unvalidated runtime environment variables remain.
npm run lint
```

**Accept when:**
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and pull request checks will enforce these rules.
</enforcement>