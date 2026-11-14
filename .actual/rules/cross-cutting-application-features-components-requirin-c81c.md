---
version: 2
name: cross-cutting-application-features-components-requirin-c81c
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Application Features Components Requiring Backend Domain"
metadata:
  actual:
    adr_id: c81c852b-1cc0-4edc-8915-1b7a5fe8e03b
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

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Application Features Components Requiring Backend Domain

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for application server route handlers, API lambda routes, data aggregation utilities, client-side forms, navigation bars, metrics dashboards, interactive status components, and build-time markdown or structured content generators requiring backend domain data.

### Rules

- **R-API-001** MUST: Application features and components requiring backend domain data, API routing, or service mutations MUST import and consume shared contracts and client interfaces directly from the @openstatus/api core library.

### Verify

```bash
# Discover and execute the repository typechecking suite to verify end-to-end type compliance with @openstatus/api contracts.
npx tsc --noEmit

# Discover and execute the dependency architecture verification script or linter
npm run lint
```

**Accept when:**
- All API interaction points, route adapters, and interactive components import contracts and client bindings from @openstatus/api.
- Static analysis and verification suites pass with zero unregistered network schemas or conflicting contract definitions.
- Resolved lockfile dependencies are verified and aligned across the workspace.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency linting and module import boundary verification checks in continuous integration, peer code reviews, and typecheck validation must pass.
</enforcement>