---
version: 2
name: cross-cutting-transactional-email-dispatch-use-resend-7809
description: "Resend Client Adoption for Transactional Email Delivery: Transactional Email Dispatch Use Resend Client"
metadata:
  actual:
    adr_id: 7809d048-491e-4c35-9827-370e68e9e45f
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Transactional Email Dispatch Use Resend Client

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RESEND-001** MUST: All transactional email dispatch MUST use the Resend client library through standardized transmission interfaces rather than custom mail transport protocols.

### Verify

```bash
# Discover and run the project's dependency audit and integrity check scripts
# Discover and run the project's static analysis and type checking suite
# Discover and run the project's unit and integration test suite
```

**Accept when:**
- All static analysis and type checking suites pass without type mismatch errors on email client method invocations.
- The dependency resolution artifact confirms the email client package matches the approved locked version.
- Automated tests confirm error logging triggers upon simulated transmission failure without crashing the host process.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>