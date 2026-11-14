---
version: 2
name: cross-cutting-consumers-discover-project-dependency-lo-0503
description: "OpenStatus Public API Direct Fetch Integration: Consumers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 05032477-9e28-4387-becc-21c5505ae54f
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Consumers Discover Project Dependency Lock Artifact

Scope: applies to `packages/react/src/**`.

These rules are ALWAYS ACTIVE for standalone user interface components requiring direct client-side external service status reporting.

### Rules

- **R-OPEN-001** MUST: Consumers MUST discover the project dependency lock artifact and verify the exact resolved version of any shared networking or rendering packages before implementation.

### Verify

```bash
# Discover and execute the project automated test script defined in the workspace manifest to validate widget fetching behavior.
# Discover and execute the repository static analysis script to verify HTTP cache configuration constraints.
```

**Accept when:**
- Automated test suites confirm client-side widget requests target the expected public endpoint with cache set to no-cache.
- Widget gracefully handles simulated external endpoint failure without throwing unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated pull request test suites, static analysis checks, and peer code review.
</enforcement>