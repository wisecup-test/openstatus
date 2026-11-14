---
version: 2
name: cross-cutting-email-dispatch-failure-logging-not-8233
description: "Resend Email Client Error Logging via Console: Email Dispatch Failure Logging Not Output"
metadata:
  actual:
    adr_id: 8233c89c-5e12-440e-bfa7-3bb5786a2cf3
    status: accepted
    candidate_group_id: ba818162-80dd-4d64-adf5-c2c7986fed07
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Email Client Error Logging via Console: Email Dispatch Failure Logging Not Output

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for email dispatch client modules handling transactional and notification email delivery.

### Rules

- **R-RESEND-001** MUST_NOT: Email dispatch failure logging MUST NOT output sensitive authentication tokens or secret credentials to unredacted output streams.

### Verify

```bash
# Discover and run the repository static analysis script from the root workspace manifest to verify logging compliance
# Discover and execute the package test suite using the project build runner to confirm error handling assertions pass
```

**Accept when:**
- All email transmission error paths are verified by automated tests to emit captured errors to the configured diagnostic sink.
- Static analysis checks pass across the workspace with zero diagnostic warnings or unhandled rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated workspace static analysis, continuous integration test suites, and pull request code reviews.
</enforcement>