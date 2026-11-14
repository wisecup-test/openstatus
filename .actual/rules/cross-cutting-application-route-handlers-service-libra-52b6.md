---
version: 2
name: cross-cutting-application-route-handlers-service-libra-52b6
description: "Adoption of @openstatus/utils Shared Utility Library: Application Route Handlers Service Libraries Not"
metadata:
  actual:
    adr_id: 52b652ff-9f6a-4313-bd7e-12be94244bad
    status: accepted
    candidate_group_id: 7e427145-5612-4048-851d-1bafe1b94f4d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/**
---

# Adoption of @openstatus/utils Shared Utility Library: Application Route Handlers Service Libraries Not

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application routes, service layers, and shared domain libraries requiring general utility, sanitization, or normalization helpers.

### Rules

- **R-UTIL-001** SHOULD_NOT: Application route handlers and service libraries SHOULD NOT introduce private utility variants for operations already supported by @openstatus/utils.

### Verify

```bash
# Discover and run the repository static analysis command from the root workspace configuration
# Discover and run the repository test execution command covering shared utility imports and consuming components
```

**Accept when:**
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of shared utility module boundaries and test execution is mandatory.
</enforcement>