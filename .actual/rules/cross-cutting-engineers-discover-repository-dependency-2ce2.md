---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-2ce2
description: "Adoption of @openstatus/db Schema Constants Module: Engineers Discover Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: 2ce21c15-5ca0-4d6e-a1ce-eaca0b622aa3
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Engineers Discover Repository Dependency Manifest Resolution

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for server routing modules, remote procedure call handlers, validation schemas, environment configurations, and converter modules interacting with domain constants or database entities.

### Rules

- **R-DEP-001** MUST: Engineers MUST discover the repository dependency manifest and resolution lock artifact to verify the exact resolved version of internal and external workspace packages prior to implementing or modifying dependent modules.

### Verify

```bash
# Discover the primary package configuration script for static type verification and execute it
# Discover the workspace test execution script and run the automated test suite
# Inspect the repository script definitions and execute the linter to verify import boundary constraints
```

**Accept when:**
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>