---
version: 2
name: cross-cutting-single-sign-service-operations-incoming-87d5
description: "Internal SSO Client Module Adoption: Single Sign Service Operations Incoming Notification"
metadata:
  actual:
    adr_id: 87d5d45c-3777-41d4-9748-ef1c0ac49983
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Single Sign Service Operations Incoming Notification

Scope: applies to `packages/services/src/sso/**`.

These rules are ALWAYS ACTIVE for all service handlers and endpoints implementing single sign-on flows, directory synchronizations, portal link generations, and webhook event processing.

### Rules

- **R-SSO-001** MUST: All single sign-on service operations and incoming notification handlers MUST mediate external identity provider interactions exclusively through the internal SSO client module.

### Verify

```bash
# Discover and execute project static analysis and linting task configurations to verify SSO import boundaries
# Discover and execute repository test runners across the service test suite
```

**Accept when:**
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. All identity provider interactions must traverse the internal client module.
</enforcement>