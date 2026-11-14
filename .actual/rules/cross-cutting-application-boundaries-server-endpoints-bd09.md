---
version: 2
name: cross-cutting-application-boundaries-server-endpoints-bd09
description: "Internal Authentication Module (@/lib/auth) Adoption: Application Boundaries Server Endpoints Execution Contexts"
metadata:
  actual:
    adr_id: bd098d54-ef36-4826-adf0-3d09b86fdbdf
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Application Boundaries Server Endpoints Execution Contexts

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AUTH-001** MUST: Application boundaries, server endpoints, and execution contexts requiring identity verification or session resolution MUST consume authentication primitives exclusively through the internal authentication module.

### Verify

```bash
# Discover workspace static analysis script and execute across route/service boundaries
# Locate repository test runner and run test suite covering authentication utilities
# Query repository imports to verify all session and identity operations originate from the internal auth module
```

**Accept when:**
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>