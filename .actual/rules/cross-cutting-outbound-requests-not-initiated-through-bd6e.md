---
version: 2
name: cross-cutting-outbound-requests-not-initiated-through-bd6e
description: "Bounded net/http Client Configuration for Internal API Boundaries: Outbound Requests Not Initiated Through Package"
metadata:
  actual:
    adr_id: bd6e244f-f4a8-4a5b-ab0c-d258363f7358
    status: accepted
    candidate_group_id: 38c879f3-bd47-4707-8de8-53d6496737aa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - handlers/**
      - pkg/job/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Bounded net/http Client Configuration for Internal API Boundaries: Outbound Requests Not Initiated Through Package

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HTTP-001** MUST_NOT: Outbound requests MUST NOT be initiated through package-level default HTTP transport or default client references that lack explicit timeout constraints.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering client boundaries.
# Discover and run the project static analysis and linting suite to check for unconfigured HTTP client usages.
```

**Accept when:**
- All outbound HTTP client instantiations declare explicit timeout properties and pass automated tests.
- No instances of unconfigured package-level default HTTP client calls exist across integration packages.
- Verification suites confirm bounded execution and context propagation across client boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>