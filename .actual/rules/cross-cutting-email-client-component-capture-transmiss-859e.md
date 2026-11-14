---
version: 2
name: cross-cutting-email-client-component-capture-transmiss-859e
description: "Resend Email Client Error Logging via Console: Email Client Component Capture Transmission Failures"
metadata:
  actual:
    adr_id: 859e865c-2d42-46b3-ab96-517eb34de288
    status: accepted
    candidate_group_id: ba818162-80dd-4d64-adf5-c2c7986fed07
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Email Client Error Logging via Console: Email Client Component Capture Transmission Failures

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RESEND-001** MUST: The email client component MUST capture transmission failures and API error responses from Resend email dispatch calls and emit them to standard diagnostic logging with the target recipient identifier.

### Verify

```bash
# Discover and run the repository static analysis script from the root workspace manifest to verify logging compliance.
# Discover and execute the package test suite using the project build runner to confirm error handling assertions pass.
```

**Accept when:**
- All email transmission error paths are verified by automated tests to emit captured errors to the configured diagnostic sink.
- Static analysis checks pass across the workspace with zero diagnostic warnings or unhandled rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by pull request code reviews and automated workspace static analysis and continuous integration test suites.
</enforcement>