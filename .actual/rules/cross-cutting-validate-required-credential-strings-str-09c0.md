---
version: 2
name: cross-cutting-validate-required-credential-strings-str-09c0
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Validate Required Credential Strings Strict Validation"
metadata:
  actual:
    adr_id: 09c07e18-d608-4bb6-8106-e210cb94785a
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Validate Required Credential Strings Strict Validation

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Validate all required credential strings with strict validation schemas before dispatching external service requests.

### Verify

```bash
# Discover workspace dependency manifest and run type-checking
# Discover test runner configuration and execute test suites for env parsing
# Discover and run linting/static analysis to verify raw env reads are restricted
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>