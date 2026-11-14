---
version: 2
name: cross-cutting-network-requests-public-status-endpoint-53b3
description: "OpenStatus Public API Direct Fetch Integration: Network Requests Public Status Endpoint Gracefully"
metadata:
  actual:
    adr_id: 53b33515-8852-4e9d-a38d-09564f4bf44b
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Network Requests Public Status Endpoint Gracefully

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for standalone user interface components requiring direct client-side external service status reporting.

### Rules

- **R-OPN-001** SHOULD: Network requests to the public status endpoint SHOULD gracefully handle transport errors and non-successful HTTP responses by presenting an offline fallback state.

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