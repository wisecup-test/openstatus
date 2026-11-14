---
version: 2
name: cross-cutting-direct-fetch-invocations-public-status-1d42
description: "OpenStatus Public API Direct Fetch Integration: Direct Fetch Invocations Public Status Endpoint"
metadata:
  actual:
    adr_id: 1d429125-84e1-4b4f-aae5-51ddb30ebd2f
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Direct Fetch Invocations Public Status Endpoint

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for standalone user interface components requiring direct client-side external service status reporting.

### Rules

- **R-OS-001** MUST: Direct fetch invocations to the public status endpoint MUST explicitly specify a cache policy of no-cache to guarantee real-time availability status.

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