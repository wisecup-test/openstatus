---
version: 2
name: cross-cutting-integration-modules-requiring-secret-cre-54db
description: "Direct process.env Access for Webhook Secret Configuration: Integration Modules Requiring Secret Credentials Access"
metadata:
  actual:
    adr_id: 54db3c67-2aab-4887-851e-66de9b42b941
    status: accepted
    candidate_group_id: b3b62330-898a-4005-92ab-1844d1a6e727
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Direct process.env Access for Webhook Secret Configuration: Integration Modules Requiring Secret Credentials Access

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all outbound notification integrations and credential-dependent delivery modules requiring destination webhook secrets.

### Rules

- **R-INT-001** MUST: Integration modules requiring secret credentials MUST access them through an explicit configuration layer that parses and validates secret values at application startup.

### Verify

```bash
# Discover and run the project repository's linting script to check for unauthorized direct environment variable access.
# Discover and execute the project's test suite to verify that configuration schema validation and credential masking pass.
```

**Accept when:**
- Direct queries to process environment variables in integration modules are replaced by validated configuration access.
- Static verification and automated test suites complete with zero errors or warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis checks in CI pipelines and peer code reviews.
</enforcement>