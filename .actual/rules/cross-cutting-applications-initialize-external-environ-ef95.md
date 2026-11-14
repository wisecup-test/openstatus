---
version: 2
name: cross-cutting-applications-initialize-external-environ-ef95
description: "godotenv Environment Configuration: Applications Initialize External Environment Loaders Exclusively"
metadata:
  actual:
    adr_id: ef95b18b-0fcf-4947-a35b-2fff77e9c7f9
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

# godotenv Environment Configuration: Applications Initialize External Environment Loaders Exclusively

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for internal service components, external API clients, and datastore initializers that consume runtime environment configuration.

### Rules

- **R-ENV-001** MUST: Applications MUST initialize external environment loaders exclusively at application startup before any dependent subsystem or client is constructed.

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