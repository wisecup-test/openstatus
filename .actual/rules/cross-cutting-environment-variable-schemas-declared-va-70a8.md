---
version: 2
name: cross-cutting-environment-variable-schemas-declared-va-70a8
description: "@t3-oss/env-core Environment Variable Schema Validation: Environment Variable Schemas Declared Validated Oss"
metadata:
  actual:
    adr_id: 70a834b0-ad2d-40fc-a5f2-12f97737ea98
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Environment Variable Schemas Declared Validated Oss

Scope: applies to `packages/analytics/**`.

These rules are ALWAYS ACTIVE for modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

### Rules

- **R-ENV-001** MUST: Environment variable schemas MUST be declared and validated using @t3-oss/env-core with zod validators at module boundary initialization.

### Verify

```bash
# Discover and execute project test script to verify environment schema parsing
npm test

# Discover and run project type checking script
npm run typecheck

# Discover and execute linting and boundary verification scripts
npm run lint
```

**Accept when:**
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, static analysis, and peer code reviews enforce these rules, and violations will block merging or cause build failures.
</enforcement>