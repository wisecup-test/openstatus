---
version: 2
name: cross-cutting-each-declared-credential-secret-constrai-3a1a
description: "@t3-oss/env-core Secret and Environment Variable Validation: Each Declared Credential Secret Constrained Appropriate"
metadata:
  actual:
    adr_id: 3a1a4ed1-2bb2-44aa-b302-e7cd56501a0f
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Each Declared Credential Secret Constrained Appropriate

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API routers, background workers, and service adapters accessing runtime secrets, API keys, or operational environment configuration.

### Rules

- **R-ENV-001** MUST: Each declared credential or secret MUST be constrained with appropriate validation schemas asserting string format and presence constraints.

### Verify

```bash
# Discover the repository verification script from the project manifest and execute the test suite to validate schema parsing
# Execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module
```

**Accept when:**
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>