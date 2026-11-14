---
version: 2
name: cross-cutting-functions-published-within-openstatus-ut-4202
description: "Adoption of @openstatus/utils Shared Utility Library: Functions Published Within Openstatus Utils Remain"
metadata:
  actual:
    adr_id: 42024ad3-54dd-4961-9778-faf3ac4bfe6b
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

# Adoption of @openstatus/utils Shared Utility Library: Functions Published Within Openstatus Utils Remain

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPEN-001** MUST: Functions published within @openstatus/utils MUST remain domain-agnostic, pure, and free of feature-specific business state.

### Verify

```bash
# Discover and run the repository static analysis command to verify module import boundaries
# Discover and run the test execution command covering shared utility imports and consuming components
```

**Accept when:**
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis checks and peer code reviews enforce clean module boundary imports and dependency lint rules.
</enforcement>