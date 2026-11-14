---
version: 2
name: cross-cutting-error-logging-mechanisms-handling-failed-d142
description: "Direct process.env Access for Webhook Secret Configuration: Error Logging Mechanisms Handling Failed Outbound"
metadata:
  actual:
    adr_id: d142cf72-158c-498c-baf0-7aab833df72c
    status: accepted
    candidate_group_id: b3b62330-898a-4005-92ab-1844d1a6e727
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Direct process.env Access for Webhook Secret Configuration: Error Logging Mechanisms Handling Failed Outbound

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for outbound notification integrations and credential-dependent delivery modules requiring destination webhook secrets.

### Rules

- **R-WEB-001** MUST_NOT: Error logging mechanisms handling failed outbound notification calls MUST NOT emit raw secret tokens or unmasked webhook uniform resource identifiers to standard diagnostic outputs.

### Verify

```bash
# Discover and run the project repository's linting script to check for unauthorized direct environment variable access.
# Discover and execute the project's test suite to verify that configuration schema validation and credential masking pass.
```

**Accept when:**
- Direct queries to process environment variables in integration modules are replaced by validated configuration access.
- Static verification and automated test suites complete with zero errors or warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>