---
version: 2
name: cross-cutting-modules-not-read-sensitive-credentials-6568
description: "Direct process.env Access for Webhook Secret Configuration: Modules Not Read Sensitive Credentials Destination"
metadata:
  actual:
    adr_id: 6568ead6-ed42-436c-ad2f-c41bfe96e57e
    status: accepted
    candidate_group_id: b3b62330-898a-4005-92ab-1844d1a6e727
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Direct process.env Access for Webhook Secret Configuration: Modules Not Read Sensitive Credentials Destination

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for outbound notification integrations and credential-dependent delivery modules requiring destination webhook secrets.

### Rules

- **R-ENV-001** MUST_NOT: Modules MUST NOT read sensitive credentials and destination secrets directly from ambient process environment variables across integration layers.

### Verify

```bash
# Discover and run the project repository's linting script to check for unauthorized direct environment variable access.
# Discover and execute the project's test suite to verify that configuration schema validation and credential masking pass.
```

**Accept when:**
- Direct queries to process environment variables in integration modules are replaced by validated configuration access.
- Static verification and automated test suites complete with zero errors or warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis checks and peer code review.
</enforcement>