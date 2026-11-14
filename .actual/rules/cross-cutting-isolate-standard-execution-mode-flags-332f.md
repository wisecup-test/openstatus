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
- **R-ENV-003** MANDATORY: Declare all required runtime secrets and configuration values in the dedicated schema definition module, exporting a validated configuration object for package consumption.
- **R-ENV-004** MANDATORY: Reference the validated configuration object in all client wrappers and dispatch logic rather than querying the host environment directly.

### Verify

```bash
# Discover and execute the workspace type-checking script
# Discover and execute project test runner validating runtime environment parsing
# Discover and run project linting/static analysis verifying raw runtime environment reads are restricted
```

**Accept when:**
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>