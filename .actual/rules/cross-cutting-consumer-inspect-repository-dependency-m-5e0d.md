---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-m-5e0d
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Consumer Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 5e0d8305-0206-4265-9725-7808e3545e3e
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Consumer Inspect Repository Dependency Manifest Authoritative

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: The consumer MUST inspect the repository dependency manifest and authoritative lock artifact to resolve the exact locked version of @t3-oss/env-core and verify its API specification before implementation.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test runner
# Discover the static analysis and type checking script to verify environment schema type inference
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated verification and peer code review audit source changes to prevent direct unvalidated environment variable access.
</enforcement>