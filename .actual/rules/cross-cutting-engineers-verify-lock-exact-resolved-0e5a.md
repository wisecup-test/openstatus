---
version: 2
name: cross-cutting-engineers-verify-lock-exact-resolved-0e5a
description: "Resend Client Adoption for Transactional Email Delivery: Engineers Verify Lock Exact Resolved Version"
metadata:
  actual:
    adr_id: 0e5a4b77-b327-44ba-9aff-21486810f088
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Engineers Verify Lock Exact Resolved Version

Scope: applies to `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for modules responsible for formatting and transmitting transactional notifications, user invitations, and engagement emails, as well as batch messaging workflows.

### Rules

- **R-RESEND-001** MUST: Engineers MUST verify and lock the exact resolved version of the Resend dependency in the project dependency resolution artifact before authoring or modifying email transmission modules.
- **R-RESEND-002** MUST: Instantiate email delivery clients using API keys sourced strictly from runtime environment configurations, never embedding credentials within client source files.
- **R-RESEND-003** SHOULD: When dispatching messages to multiple recipients, prefer batch sending interfaces over serial single-message dispatches to minimize network overhead.

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