---
version: 2
name: cross-cutting-developers-discover-project-dependency-m-bd38
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Developers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: bd38274d-95b3-4d13-a99c-131caf01ffc4
    status: accepted
    candidate_group_id: 6a007d6a-909a-425c-8c5a-15faed6eeac3
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Developers Discover Project Dependency Manifest Lock

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all application endpoints, route handlers, webhook receivers, client components, server rendering components, and metadata generators invoking or defining remote procedures or querying domain entities via internal API interfaces.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project dependency manifest and lock artifact to resolve and verify the authoritative version of internal and external dependencies prior to usage.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify that all procedure calls match module contracts.
# Discover and run the workspace test suite to ensure server route handlers and client callers integrate without contract violations.
```

**Accept when:**
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must be verified against the workspace type checking and test suites.
</enforcement>