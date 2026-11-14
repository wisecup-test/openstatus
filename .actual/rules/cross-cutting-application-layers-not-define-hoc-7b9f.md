---
version: 2
name: cross-cutting-application-layers-not-define-hoc-7b9f
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Application Layers Not Define Hoc Duplicate"
metadata:
  actual:
    adr_id: 7b9f6dd6-b492-4698-8fcc-ab457aa32ae2
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

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Application Layers Not Define Hoc Duplicate

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Application layers MUST NOT define ad-hoc duplicate network request contracts or decoupled endpoint payload schemas that bypass the @openstatus/api definitions.

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
Claude Code MUST NOT skip or defer verification. All API interactions must use @openstatus/api contracts and verify type compliance via CI check suites.
</enforcement>