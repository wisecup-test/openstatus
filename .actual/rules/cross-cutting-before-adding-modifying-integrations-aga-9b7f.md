---
version: 2
name: cross-cutting-before-adding-modifying-integrations-aga-9b7f
description: "Internal SSO Client Module Adoption: Before Adding Modifying Integrations Against Versioned"
metadata:
  actual:
    adr_id: 9b7f1665-fef1-43b3-9cf1-8a61da59c244
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Before Adding Modifying Integrations Against Versioned

Scope: applies to `packages/services/src/sso/**`.

These rules are ALWAYS ACTIVE for service handlers and endpoints implementing single sign-on flows, directory synchronizations, portal link generations, and webhook event processing.

### Rules

- **R-SSO-001** MUST: Before adding or modifying integrations against versioned external client libraries that underpin the internal client module, consumers MUST verify resolved versions against the project lock resolution artifact in accordance with the lock-version grounding policy.

### Verify

```bash
# Discover and execute repository test runners across the service test suite to validate that identity workflows execute with expected client mocking and context propagation.
# Discover the project static analysis and linting task configurations to verify that service handlers import identity client capabilities strictly from the authorized internal client module.
```

**Accept when:**
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>