---
version: 2
name: cross-cutting-static-content-generators-directly-invok-22e4
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Static Content Generators Directly Invoke Query"
metadata:
  actual:
    adr_id: 22e45765-569d-4e3a-bb86-21db80967004
    status: accepted
    candidate_group_id: 680fce54-561c-430b-a8f6-11b8816f240e
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/api/trpc/lambda/**
      - src/components/**
      - src/content/**
      - src/data/**
      - src/lib/trpc/**
---

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Static Content Generators Directly Invoke Query

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-API-001** MAY: Static content generators MAY directly invoke query utilities and schema parsers from @openstatus/api when assembling build-time artifacts.

### Verify

```bash
# Discover and execute the repository typechecking suite to verify end-to-end type compliance with @openstatus/api contracts.
# Discover and execute the dependency architecture verification script to detect unapproved network fetch calls or unauthorized external schema definitions.
```

**Accept when:**
- All API interaction points, route adapters, and interactive components import contracts and client bindings from @openstatus/api.
- Static analysis and verification suites pass with zero unregistered network schemas or conflicting contract definitions.
- Resolved lockfile dependencies are verified and aligned across the workspace.

<enforcement>
Claude Code MUST NOT skip or defer verification. All API interactions, contracts, and service integrations must comply with @openstatus/api core library usage policies.
</enforcement>