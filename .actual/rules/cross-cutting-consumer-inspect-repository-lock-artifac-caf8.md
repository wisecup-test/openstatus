---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-caf8
description: "godotenv Environment Configuration: Consumer Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: caf88c90-01dd-458a-877b-7b6b10fdeee3
    status: accepted
    candidate_group_id: ad5408b7-dbfd-46f7-8b98-2fb9f5252524
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/database/**
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
---

# godotenv Environment Configuration: Consumer Inspect Repository Lock Artifact Verify

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for internal service components, external API clients, and datastore initializers that consume runtime environment configuration.

### Rules

- **R-ENV-001** MUST: The consumer MUST inspect the repository lock artifact to verify the exact resolved version of any runtime configuration library before introducing or modifying configuration bindings.

### Verify

```bash
# Discover and execute the project static analysis and linting scripts to identify unmanaged environment lookups.
# Discover and run the project automated test suite to ensure configuration parsing functions conform to expectations.
```

**Accept when:**
- Static analysis checks report no unauthorized direct environment access calls in client packages.
- Automated test suites pass with validated configuration inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>