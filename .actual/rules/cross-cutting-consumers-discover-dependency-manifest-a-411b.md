---
version: 2
name: cross-cutting-consumers-discover-dependency-manifest-a-411b
description: "Fetch API Client Boundary Integration: Consumers Discover Dependency Manifest Authoritative Resolution"
metadata:
  actual:
    adr_id: 411bf326-ae3e-4a97-a89b-7c3dd9db18d9
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

# Fetch API Client Boundary Integration: Consumers Discover Dependency Manifest Authoritative Resolution

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all client-side components, hooks, server-side utility functions, background checkers, and route handlers communicating across external HTTP boundaries.

### Rules

- **R-FETCH-001** MUST: Consumers MUST discover the dependency manifest and authoritative resolution lock artifact to verify runtime compatibility and exact versions of all supporting libraries before implementation.

### Verify

```bash
# Discover and run the project static analysis and linting command from the root configuration.
# Discover and execute the project test runner across all integration test suites.
# Discover and execute the project type checking script from the workspace manifest.
```

**Accept when:**
- Static analysis validates that all boundary network requests conform to the standard client rules without unapproved HTTP dependencies.
- All integration test suites covering internal routes and external boundary clients pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>