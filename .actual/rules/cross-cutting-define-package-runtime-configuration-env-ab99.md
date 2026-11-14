---
version: 2
name: cross-cutting-define-package-runtime-configuration-env-ab99
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Define Package Runtime Configuration Environment Secret"
metadata:
  actual:
    adr_id: ab994a52-d307-456d-86f4-5788fe099017
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Define Package Runtime Configuration Environment Secret

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Define all package runtime configuration and environment secret access through schemas defined using @t3-oss/env-core with Zod validation.

### Verify

```bash
# Discover workspace dependency manifest and execute type-checking script
# Discover project test runner configuration and execute test suites validating runtime environment variable parsing
# Discover and run project linting and static analysis scripts to verify raw runtime environment reads are restricted
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and static analysis checks enforce this policy.
</enforcement>