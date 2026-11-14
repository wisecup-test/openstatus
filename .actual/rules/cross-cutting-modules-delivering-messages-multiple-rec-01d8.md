---
version: 2
name: cross-cutting-modules-delivering-messages-multiple-rec-01d8
description: "Resend Client Adoption for Transactional Email Delivery: Modules Delivering Messages Multiple Recipients Invoke"
metadata:
  actual:
    adr_id: 01d89725-dc77-44fe-91d7-6cc825f89c33
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Modules Delivering Messages Multiple Recipients Invoke

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for modules delivering messages to multiple recipients.

### Rules

- **R-RESEND-001** MUST: Modules delivering messages to multiple recipients MUST invoke batch.send or the corresponding batch delivery endpoint to execute batched dispatches in a single network transaction.

### Verify

```bash
# Discover and run the project's dependency audit and integrity check scripts to verify the email client library matches the locked manifest version.
# Discover and run the project's static analysis and type checking suite to confirm email client imports and method invocations conform to client signatures.
# Discover and run the project's unit and integration test suite to validate mock email dispatch behaviors, error handling paths, and environment credential resolution.
```

**Accept when:**
- All static analysis and type checking suites pass without type mismatch errors on email client method invocations.
- The dependency resolution artifact confirms the email client package matches the approved locked version.
- Automated tests confirm error logging triggers upon simulated transmission failure without crashing the host process.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>