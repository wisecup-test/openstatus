---
version: 2
name: cross-cutting-service-endpoint-handlers-not-instantiat-11d5
description: "Internal SSO Client Module Adoption: Service Endpoint Handlers Not Instantiate Direct"
metadata:
  actual:
    adr_id: 11d5bd87-de67-4e74-8949-a70e798e40e8
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Service Endpoint Handlers Not Instantiate Direct

Scope: applies to `packages/services/src/sso/**`.

These rules are ALWAYS ACTIVE for service handlers and endpoints implementing single sign-on flows, directory synchronizations, portal link generations, and webhook event processing.

### Rules

- **R-SSO-001** MUST_NOT: Service endpoint handlers MUST NOT instantiate direct network transport clients, construct ad-hoc identity protocol requests, or access raw third-party identity APIs outside the designated internal client module.

### Verify

```bash
# Discover and execute project static analysis and linting tasks to verify import boundaries
# Discover and execute repository test runners across the service test suite
```

**Accept when:**
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>