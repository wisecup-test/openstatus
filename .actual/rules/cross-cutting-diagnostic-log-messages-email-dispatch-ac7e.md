---
version: 2
name: cross-cutting-diagnostic-log-messages-email-dispatch-ac7e
description: "Resend Email Client Error Logging via Console: Diagnostic Log Messages Email Dispatch Failures"
metadata:
  actual:
    adr_id: ac7e322a-3fd1-4177-a3ed-b64ab84e5263
    status: accepted
    candidate_group_id: ba818162-80dd-4d64-adf5-c2c7986fed07
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Email Client Error Logging via Console: Diagnostic Log Messages Email Dispatch Failures

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RESEND-LOG-001** SHOULD: Diagnostic log messages for email dispatch failures SHOULD include structured context containing the dispatch operation type and error payload rather than concatenated plain strings.

### Verify

```bash
# Discover and run the repository static analysis script from the root workspace manifest
# Discover and execute the package test suite using the project build runner
```

**Accept when:**
- All email transmission error paths are verified by automated tests to emit captured errors to the configured diagnostic sink.
- Static analysis checks pass across the workspace with zero diagnostic warnings or unhandled rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>