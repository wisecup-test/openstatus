---
version: 2
name: cross-cutting-server-route-handlers-data-access-93cb
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Server Route Handlers Data Access Layers"
metadata:
  actual:
    adr_id: 93cb1e47-d842-4f62-bac3-d7a7c4e57712
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

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Server Route Handlers Data Access Layers

Scope: applies to `apps/status-page/src/app/api/trpc/lambda/**`, `apps/status-page/src/components/**`, `apps/status-page/src/content/**`, `apps/status-page/src/data/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** SHOULD: Server route handlers and data access layers SHOULD expose error handling structures utilizing the dedicated error definitions exported by @openstatus/api.
- **R-OPENSTATUS-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-OPENSTATUS-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation/changelog for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-OPENSTATUS-004** MANDATORY: Ensure module import paths adhere to the public export boundaries defined by the core library to preserve encapsulation.
- **R-OPENSTATUS-005** MANDATORY: Structure client-side reactive consumers to use shared router bindings rather than manual request invocations.

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
Claude Code MUST NOT skip or defer verification. Automated dependency linting, module import boundary verification checks, and peer code review enforce these rules, resulting in build failure or review rejection upon violation.
</enforcement>