---
version: 2
name: cross-cutting-subsystem-client-components-not-perform-1b6b
description: "godotenv Environment Configuration: Subsystem Client Components Not Perform Hoc"
metadata:
  actual:
    adr_id: 1b6b9047-cf66-4fc1-8838-d2e305068472
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

# godotenv Environment Configuration: Subsystem Client Components Not Perform Hoc

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for internal service components, external API clients, and datastore initializers that consume runtime environment configuration.

### Rules

- **R-ENV-001** MUST_NOT: Subsystem client components MUST NOT perform ad-hoc lookups of ambient environment variables directly within internal constructor functions.

### Verify

```bash
# Discover and execute project static analysis, linting, and test suites to verify configuration patterns
# (Tooling and commands are project-dependent; inspect the repository to locate analysis and test execution scripts)
```

**Accept when:**
- Static analysis checks report no unauthorized direct environment access calls in client packages.
- Automated test suites pass with validated configuration inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>