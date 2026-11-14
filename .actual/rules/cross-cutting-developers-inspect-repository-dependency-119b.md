---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-119b
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Developers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: 119b4b2c-63de-4d4b-b17f-72500654beea
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Developers Inspect Repository Dependency Manifest Resolution

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for Server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-ADR-001** MUST: Developers MUST inspect the repository dependency manifest and resolution lock artifact to confirm the exact resolved version of internal and external workspace packages prior to implementing schema extensions.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# Discover and run the project test runner across all limit handler test suites
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>