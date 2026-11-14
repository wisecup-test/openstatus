---
version: 2
name: cross-cutting-interactive-client-components-consume-re-9d49
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Interactive Client Components Consume Reactive Data"
metadata:
  actual:
    adr_id: 9d492569-2ac6-4de4-838b-dc565fa5f9bd
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

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Interactive Client Components Consume Reactive Data

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPEN-001** SHOULD: Interactive client components SHOULD consume reactive data fetching and mutation hooks derived from @openstatus/api router configurations rather than issuing raw HTTP fetch calls.

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
Claude Code MUST NOT skip or defer verification. Automated dependency linting, module import boundary verification checks, and typecheck validation are enforced in continuous integration.
</enforcement>