---
version: 2
name: cross-cutting-modules-transmitting-individual-messages-2bb4
description: "Resend Client Adoption for Transactional Email Delivery: Modules Transmitting Individual Messages Invoke Client"
metadata:
  actual:
    adr_id: 2bb436e5-ebc9-4976-99fc-d1989cec2fd7
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Modules Transmitting Individual Messages Invoke Client

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for modules responsible for formatting and transmitting transactional notifications, user invitations, and engagement emails.

### Rules

- **R-RESEND-001** MUST: Modules transmitting individual messages MUST invoke the client's emails.send method with validated recipient and sender specifications.

### Verify

```bash
# Discover and run dependency audit/integrity checks to verify email client matches locked manifest version
# Discover and run static analysis and type checking suite
# Discover and run unit and integration test suite
```

**Accept when:**
- All static analysis and type checking suites pass without type mismatch errors on email client method invocations.
- The dependency resolution artifact confirms the email client package matches the approved locked version.
- Automated tests confirm error logging triggers upon simulated transmission failure without crashing the host process.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>