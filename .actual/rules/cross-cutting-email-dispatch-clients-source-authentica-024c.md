---
version: 2
name: cross-cutting-email-dispatch-clients-source-authentica-024c
description: "Resend Client Adoption for Transactional Email Delivery: Email Dispatch Clients Source Authentication Credentials"
metadata:
  actual:
    adr_id: 024c4385-aca9-4268-aa92-51f7ac1826ab
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Email Dispatch Clients Source Authentication Credentials

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all modules responsible for formatting and transmitting transactional notifications, user invitations, and engagement emails, as well as batch messaging workflows sending communication to multiple recipients simultaneously.

### Rules

- **R-RESEND-001** MUST: Email dispatch clients MUST source authentication credentials and operational settings from validated runtime environment configurations rather than hardcoded credentials.

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