---
version: 2
name: cross-cutting-modules-requiring-general-purpose-utilit-6a7e
description: "Adoption of @openstatus/utils Shared Utility Library: Modules Requiring General Purpose Utility Functions"
metadata:
  actual:
    adr_id: 6a7e08a1-eb83-46ae-ae08-4e2342836cb3
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

# Adoption of @openstatus/utils Shared Utility Library: Modules Requiring General Purpose Utility Functions

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application routes, service layers, and shared domain libraries requiring general utility, sanitization, or normalization helpers.

### Rules

- **R-UTIL-001** MUST: Modules requiring general-purpose utility functions, URL normalization, or cross-cutting helper logic MUST import them from @openstatus/utils rather than implementing local duplicate helpers.

### Verify

```bash
# Discover and run the repository static analysis command to verify module import boundaries
# Discover and run the test execution command covering shared utility imports and consuming components
```

**Accept when:**
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>