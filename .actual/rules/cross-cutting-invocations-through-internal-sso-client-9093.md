---
version: 2
name: cross-cutting-invocations-through-internal-sso-client-9093
description: "Internal SSO Client Module Adoption: Invocations Through Internal Sso Client Module"
metadata:
  actual:
    adr_id: 90939b2f-1e0c-4207-b314-cf7bacb0756e
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Invocations Through Internal Sso Client Module

Scope: applies to `packages/services/src/sso/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SSO-001** MUST: Invocations through the internal SSO client module MUST propagate execution context and error mappings established by shared service boundary utilities.

### Verify

```bash
# Discover and execute project static analysis, linting tasks, and repository test runners
# to verify that service handlers import identity client capabilities strictly from
# the authorized internal client module and execute with expected mocking/context propagation.
```

**Accept when:**
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. All identity provider interactions must traverse the internal client module, and direct external client instantiations or unapproved transport calls within service handlers are blocked from merging.
</enforcement>