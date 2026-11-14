---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-aadd
description: "Adoption of @openstatus/error for Server Error Standardization: Developers Inspect Repository Dependency Resolution Lock"
metadata:
  actual:
    adr_id: aaddbecd-6a58-43d1-92b0-2b9d8a56aa10
    status: accepted
    candidate_group_id: f4bfe830-6739-435c-8463-26208c562aeb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/errors/**
      - src/routes/rpc/interceptors/**
---

# Adoption of @openstatus/error for Server Error Standardization: Developers Inspect Repository Dependency Resolution Lock

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all server route handlers, RPC procedure implementations, transport interceptors emitting error responses, and error utility modules defining API error schemas and response structures.

### Rules

- **R-ERR-001** MUST: Developers MUST inspect the repository dependency resolution lock artifact to identify and verify the exact resolved version of @openstatus/error before introducing or updating error handling implementations.

### Verify

```bash
# Discover and run type-checking across server packages
# (Command must be derived from repository manifest and build tooling)

# Execute unit and integration test suite

# Execute linting and static analysis suite
```

**Accept when:**
- Type checking passes without errors, confirming all referenced error codes and schema objects conform to @openstatus/error definitions.
- Unit and integration tests pass, confirming HTTP and RPC error responses emit standardized error codes and correlation identifiers.
- Static analysis validates that unexpected errors are intercepted and logged with correlated request identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>