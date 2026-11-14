---
version: 2
name: cross-cutting-before-introducing-modifying-versioned-d-98c7
description: "Direct process.env Access for Webhook Secret Configuration: Before Introducing Modifying Versioned Dependency Packages"
metadata:
  actual:
    adr_id: 98c7ae13-0d11-4bd7-9320-1b4c1eb3e986
    status: accepted
    candidate_group_id: b3b62330-898a-4005-92ab-1844d1a6e727
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Direct process.env Access for Webhook Secret Configuration: Before Introducing Modifying Versioned Dependency Packages

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for outbound notification integrations and credential-dependent delivery modules requiring destination webhook secrets.

### Rules

- **R-WEB-001** MUST: Before introducing or modifying versioned dependency packages for secret handling or schema validation, developers MUST discover the workspace lock artifact and ensure exact resolved version parity.

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