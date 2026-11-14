---
version: 2
name: cross-cutting-consumer-modules-requiring-domain-consta-94b7
description: "Adoption of @openstatus/db Schema Constants Module: Consumer Modules Requiring Domain Constants Not"
metadata:
  actual:
    adr_id: 94b7b706-9142-4ea1-9a97-0a5e0770843b
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Consumer Modules Requiring Domain Constants Not

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD_NOT: Consumer modules requiring domain constants SHOULD NOT import unexported internal database implementation details beyond the designated schema and constants entry points of @openstatus/db.

### Verify

```bash
# Discover and execute the primary package configuration script for static type verification
pnpm typecheck || npm run typecheck

# Discover and execute the workspace test suite against consuming packages
pnpm test || npm run test

# Execute the linter to verify import boundary constraints
pnpm lint || npm run lint
```

**Accept when:**
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and peer code review.
</enforcement>