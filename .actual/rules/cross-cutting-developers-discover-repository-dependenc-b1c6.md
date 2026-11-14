---
version: 2
name: cross-cutting-developers-discover-repository-dependenc-b1c6
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Discover Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: b1c6edc2-ba64-4ff5-bed5-23d09aa0b203
    status: accepted
    candidate_group_id: 1700c304-66aa-4383-b313-bb2df6cb6fc3
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
---

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Discover Repository Dependency Manifest Lock

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the repository dependency manifest and lock artifact to verify the exact resolved runtime library version before using detached context APIs.

### Verify

```bash
# Discover and execute the repository test runner defined in the project build configuration
# Execute static analysis and linting checks discovered from the repository manifest
```

**Accept when:**
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>