---
version: 2
name: cross-cutting-domain-specific-transformations-such-dir-95d8
description: "Internal SSO Client Module Adoption: Domain Specific Transformations Such Directory Synchronization"
metadata:
  actual:
    adr_id: 95d8a536-642d-4c50-b64a-3bfcc918a546
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Domain Specific Transformations Such Directory Synchronization

Scope: applies to `packages/services/src/sso/**`.

These rules are ALWAYS ACTIVE for service handlers and endpoints implementing single sign-on flows, directory synchronizations, portal link generations, and webhook event processing.

### Rules

- **R-SSO-001** SHOULD: Domain-specific transformations, such as directory synchronization or portal link creation, SHOULD reside in specialized internal service helpers that consume the unified client interface.

### Verify

```bash
# Discover and run the project static analysis, linting, and test tasks to verify import boundaries and identity workflow executions.
```

**Accept when:**
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. All identity provider interactions must traverse the internal client module and be verified via automated static analysis and test execution.
</enforcement>