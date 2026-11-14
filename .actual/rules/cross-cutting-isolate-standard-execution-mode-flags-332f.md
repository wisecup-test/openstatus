---
version: 2
name: cross-cutting-isolate-standard-execution-mode-flags-332f
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Isolate Standard Execution Mode Flags Schema"
metadata:
  actual:
    adr_id: 332f77d5-8b67-4f43-8966-974e4c500d09
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Isolate Standard Execution Mode Flags Schema

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Isolate standard execution mode flags into schema definitions to provide consistent default fallbacks across runtime contexts.
- **R-ENV-002** MANDATORY: Discover the workspace dependency manifest and execute the designated type-checking script to confirm schema definitions compile without errors.
- **R-ENV-003** MANDATORY: Execute the discovery-based test suites and static analysis scripts to verify that raw runtime environment reads are restricted to designated configuration schemas.

### Verify

```bash
# Discover workspace dependency manifest, lockfile, and test runner/typecheck/lint scripts, then execute them:
# 1. Type check
# 2. Test runner
# 3. Static analysis / linting for unvalidated runtime environment reads
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and static analysis checks must be successfully verified.
</enforcement>