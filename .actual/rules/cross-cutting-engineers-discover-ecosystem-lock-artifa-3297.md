---
version: 2
name: cross-cutting-engineers-discover-ecosystem-lock-artifa-3297
description: "Internal Authentication Module (@/lib/auth) Adoption: Engineers Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: 3297ee94-fe47-41fd-a5f5-b6193446e8bc
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Engineers Discover Ecosystem Lock Artifact Verify

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, server actions, background task contexts, edge proxy utilities, components, and layouts requiring identity resolution or session access.

### Rules

- **R-AUTH-001** MUST: Engineers MUST discover the ecosystem lock artifact and verify the exact resolved versions of underlying security and session management packages before implementing or modifying authentication interfaces.

### Verify

```bash
# Discover the workspace static analysis script from repository configuration and execute it across all route and service boundaries.
# Locate the repository test runner from project configuration and run the test suite covering authentication utilities and dependent route handlers.
# Query repository imports to verify all session and identity operations originate from the designated internal authentication module.
```

**Accept when:**
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct external authentication imports or unapproved bypasses will be blocked from merging.
</enforcement>