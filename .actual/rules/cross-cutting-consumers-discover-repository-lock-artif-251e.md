---
version: 2
name: cross-cutting-consumers-discover-repository-lock-artif-251e
description: "Resend Email Client Error Logging via Console: Consumers Discover Repository Lock Artifact Inspect"
metadata:
  actual:
    adr_id: 251e6d54-182e-4895-ac2f-9eec0b991680
    status: accepted
    candidate_group_id: ba818162-80dd-4d64-adf5-c2c7986fed07
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Email Client Error Logging via Console: Consumers Discover Repository Lock Artifact Inspect

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RESEND-001** MUST: Consumers MUST discover the repository lock artifact and inspect the exact resolved version of Resend and related email libraries before implementing or modifying email dispatch routines.

### Verify

```bash
# Discover and run the repository static analysis script from the root workspace manifest to verify logging compliance
# Discover and execute the package test suite using the project build runner to confirm error handling assertions pass
```

**Accept when:**
- All email transmission error paths are verified by automated tests to emit captured errors to the configured diagnostic sink.
- Static analysis checks pass across the workspace with zero diagnostic warnings or unhandled rejection errors.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>