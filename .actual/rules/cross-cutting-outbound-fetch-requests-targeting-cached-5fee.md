---
version: 2
name: cross-cutting-outbound-fetch-requests-targeting-cached-5fee
description: "Fetch API Client Boundary Integration: Outbound Fetch Requests Targeting Cached Deterministic"
metadata:
  actual:
    adr_id: 5feeefff-4a25-4617-8e7c-b99fc082ee39
    status: accepted
    candidate_group_id: 57dd9a6d-921b-457c-a0a1-87439126474c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Fetch API Client Boundary Integration: Outbound Fetch Requests Targeting Cached Deterministic

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FAB-001** SHOULD: Outbound fetch requests targeting cached or deterministic resources specify explicit cache and revalidation controls matching the freshness requirements of the consumer.

### Verify

```bash
# Discover and run the project static analysis, type checking, and linting commands from the root configuration.
# Discover and execute the project test runner across all integration test suites.
```

**Accept when:**
- Static analysis validates that all boundary network requests conform to the standard client rules without unapproved HTTP dependencies.
- All integration test suites covering internal routes and external boundary clients pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>