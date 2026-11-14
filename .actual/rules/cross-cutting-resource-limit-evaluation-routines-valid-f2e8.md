---
version: 2
name: cross-cutting-resource-limit-evaluation-routines-valid-f2e8
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Resource Limit Evaluation Routines Validate Incoming"
metadata:
  actual:
    adr_id: f2e8d2a1-7008-4955-bf67-c44676cbc67b
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Resource Limit Evaluation Routines Validate Incoming

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-LIMIT-001** MUST: Resource limit evaluation routines MUST validate incoming quota parameters and structural definitions against the shared schema before completing RPC requests.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# (Command to be discovered from package.json scripts or workspace configuration)
npm run check
# Or typecheck/lint depending on repository configuration

# Discover and run the project test runner across all limit handler test suites
npm test
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace static analysis and test validation, as well as architectural and peer code reviews.
</enforcement>