---
version: 2
name: cross-cutting-client-components-not-transmit-sensitive-c433
description: "OpenStatus Public API Direct Fetch Integration: Client Components Not Transmit Sensitive Authentication"
metadata:
  actual:
    adr_id: c433d285-762d-44c7-b74e-67af98f19fe0
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Client Components Not Transmit Sensitive Authentication

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for standalone user interface components requiring direct client-side external service status reporting.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Client components MUST NOT transmit sensitive authentication tokens or credentials when querying public status endpoints.

### Verify

```bash
# Discover and execute the project automated test script defined in the workspace manifest to validate widget fetching behavior.
# Discover and execute the repository static analysis script to verify HTTP cache configuration constraints.
```

**Accept when:**
- Automated test suites confirm client-side widget requests target the expected public endpoint with cache set to no-cache.
- Widget gracefully handles simulated external endpoint failure without throwing unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>