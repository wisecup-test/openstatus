---
version: 2
name: cross-cutting-environment-variable-schemas-enforce-str-35b2
description: "@t3-oss/env-core for Environment Variable Validation: Environment Variable Schemas Enforce Structural Constraints"
metadata:
  actual:
    adr_id: 35b2b959-6495-4a7b-95f4-c3119c68ab31
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Environment Variable Schemas Enforce Structural Constraints

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for modules defining runtime configuration and external service integration credentials.

### Rules

- **R-ENV-001** SHOULD: Environment variable schemas SHOULD enforce structural constraints such as non-empty string validation for required service identifiers.

### Verify

```bash
# Discover and execute the project test runner across all packages containing environment configuration modules
# Discover and run the project type-checking and linting suites to ensure all environment variable usages reference the validated configuration object
```

**Accept when:**
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, test suites, and peer code reviews enforce these requirements, and pull requests containing direct unvalidated environment variable access or failing schema validation tests are blocked from merging.
</enforcement>