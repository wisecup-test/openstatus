---
version: 2
name: cross-cutting-configuration-values-validated-applicati-3089
description: "godotenv Environment Configuration: Configuration Values Validated Application Startup Passed"
metadata:
  actual:
    adr_id: 3089d667-2c6b-46ba-a19c-f66c8f95e7e6
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

# godotenv Environment Configuration: Configuration Values Validated Application Startup Passed

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for internal service components, external API clients, and datastore initializers that consume runtime environment configuration.

### Rules

- **R-CFG-001** SHOULD: Configuration values SHOULD be validated at application startup and passed explicitly to dependent clients via explicit configuration structures.

### Verify

```bash
# Discover and execute project static analysis and linting scripts to identify unmanaged environment lookups
# Discover and run automated test suites to ensure configuration parsing functions conform to expectations
```

**Accept when:**
- Static analysis checks report no unauthorized direct environment access calls in client packages.
- Automated test suites pass with validated configuration inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration static analysis pipelines and peer reviews.
</enforcement>