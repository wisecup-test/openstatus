---
version: 2
name: cross-cutting-email-transmission-calls-handle-dispatch-62f3
description: "Resend Client Adoption for Transactional Email Delivery: Email Transmission Calls Handle Dispatch Rejections"
metadata:
  actual:
    adr_id: 62f30834-00a2-48d2-b520-5ea16aedd57d
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Email Transmission Calls Handle Dispatch Rejections

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for modules responsible for formatting and transmitting transactional notifications, user invitations, engagement emails, and batch messaging workflows.

### Rules

- **R-RESEND-001** MUST: Email transmission calls MUST handle dispatch rejections and capture resulting errors through structured error logging.
- **R-RESEND-002** MUST: Instantiate email delivery clients using API keys sourced strictly from runtime environment configurations, never embedding credentials within client source files.
- **R-RESEND-003** SHOULD: When dispatching messages to multiple recipients, prefer batch sending interfaces over serial single-message dispatches to minimize network overhead.

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