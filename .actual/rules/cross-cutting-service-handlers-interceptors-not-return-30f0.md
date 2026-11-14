---
version: 2
name: cross-cutting-service-handlers-interceptors-not-return-30f0
description: "Adoption of @openstatus/error for Server Error Standardization: Service Handlers Interceptors Not Return Unformatted"
metadata:
  actual:
    adr_id: 30f01f00-3ad1-4feb-9b7d-b9fecd18d95b
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

# Adoption of @openstatus/error for Server Error Standardization: Service Handlers Interceptors Not Return Unformatted

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST_NOT: Service handlers and interceptors MUST NOT return unformatted raw exceptions or ad-hoc error structures that bypass @openstatus/error standards to external callers.

### Verify

```bash
# Discover and run the repository type-checking script across server packages
# Discover and execute the repository unit and integration test suite
# Execute the repository linting and static analysis suite
```

**Accept when:**
- Type checking passes without errors, confirming all referenced error codes and schema objects conform to @openstatus/error definitions.
- Unit and integration tests pass, confirming HTTP and RPC error responses emit standardized error codes and correlation identifiers.
- Static analysis validates that unexpected errors are intercepted and logged with correlated request identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing raw, unclassified error responses or bypassing @openstatus/error schemas are blocked from merging.
</enforcement>