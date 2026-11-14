---
version: 2
name: cross-cutting-access-runtime-environment-properties-di-3e9b
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Access Runtime Environment Properties Directly Domain"
metadata:
  actual:
    adr_id: 3e9b4204-4115-47bc-acd0-9c38fcb9f338
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Access Runtime Environment Properties Directly Domain

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Access runtime environment properties directly in domain logic or outbound service clients when typed configuration schema properties exist.

### Verify

```bash
# Discover the workspace dependency manifest and execute the designated type-checking script to confirm schema definitions compile without errors.
# Discover the project test runner configuration and execute test suites validating runtime environment variable parsing and failure behavior.
# Discover and run the project linting and static analysis scripts to verify that raw runtime environment reads are restricted to designated configuration schemas.
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>