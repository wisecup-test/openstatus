---
version: 2
name: cross-cutting-client-side-status-widget-integrations-ab67
description: "OpenStatus Public API Direct Fetch Integration: Client Side Status Widget Integrations Query"
metadata:
  actual:
    adr_id: ab67631b-b163-4edd-9599-cf47ea71f3d0
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Client Side Status Widget Integrations Query

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for standalone user interface components requiring direct client-side external service status reporting.

### Rules

- **R-OPN-001** MUST: Client-side status widget integrations MUST query the OpenStatus public API status endpoint directly using the platform fetch interface without intermediate server-side proxy layers.

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