---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-5f09
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Engineers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 5f09a439-55ea-49e2-a3d2-aadabdf51907
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

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Engineers Discover Repository Dependency Manifest Inspect

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-API-001** MUST: Engineers MUST discover the repository dependency manifest and inspect the authoritative lock artifact to verify the exact resolved version of all shared workspace and external dependencies prior to implementation.

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
Claude Code MUST NOT skip or defer verification. Automated dependency linting, module import boundary verification checks, and typecheck validation are mandatory in continuous integration.
</enforcement>