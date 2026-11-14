---
version: 2
name: cross-cutting-inspect-repository-lock-artifact-verify-f7c1
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Inspect Repository Lock Artifact Verify Exact"
metadata:
  actual:
    adr_id: f7c184de-f22b-4fc7-b511-254bff686ed5
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Inspect Repository Lock Artifact Verify Exact

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Inspect the repository lock artifact to verify the exact resolved version of the environment validation library before implementing or updating schema definitions.

### Verify

```bash
# Discover workspace dependency manifest and execute type-checking script
# Discover project test runner configuration and execute test suites validating environment parsing
# Discover and run project linting and static analysis scripts to verify raw environment reads are restricted
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites, static analysis checks, and peer reviews verify compliance, resulting in build rejection on violations.
</enforcement>