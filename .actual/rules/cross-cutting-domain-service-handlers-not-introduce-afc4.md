---
version: 2
name: cross-cutting-domain-service-handlers-not-introduce-afc4
description: "Internal Authentication Module (@/lib/auth) Adoption: Domain Service Handlers Not Introduce Circular"
metadata:
  actual:
    adr_id: afc4ff7a-b5b1-417b-a156-33634c9238aa
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Domain Service Handlers Not Introduce Circular

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, server actions, background task contexts, and edge proxy utilities requiring identity resolution or session access.

### Rules

- **R-AUTH-001** SHOULD_NOT: Domain service handlers SHOULD NOT introduce circular dependencies back into the authentication module when providing workspace or database context.

### Verify

```bash
# Discover the workspace static analysis script from repository configuration and execute it across all route and service boundaries.
# Locate the repository test runner from project configuration and run the test suite covering authentication utilities and dependent route handlers.
# Query repository imports to verify all session and identity operations originate from the designated internal authentication module.
```

**Accept when:**
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification. All domain service handlers and route implementations must be verified against circular dependency rules.
</enforcement>