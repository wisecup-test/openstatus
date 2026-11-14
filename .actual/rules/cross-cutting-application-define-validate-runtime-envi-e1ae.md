---
version: 2
name: cross-cutting-application-define-validate-runtime-envi-e1ae
description: "@t3-oss/env-core for Environment Variable Validation: Application Define Validate Runtime Environment Configuration"
metadata:
  actual:
    adr_id: e1aef982-cec7-4c32-bd90-d83b697d3a43
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Application Define Validate Runtime Environment Configuration

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for modules defining runtime configuration and external service integration credentials.

### Rules

- **R-ENV-001** MUST: The application MUST define and validate runtime environment configuration through @t3-oss/env-core schema specifications.
- **R-ENV-002** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute the version grounding sequence: find dependency manifest, identify build tool, inspect lock/resolution artifact for exact version, look up official docs for that exact version, confirm API existence, and re-run per dependency at point of use.
- **R-ENV-003** MANDATORY (DISCOVERY POLICY): Omit all tool names, file names, commands, package managers, and version numbers; the consumer MUST derive them from the project repository.

### Verify

```bash
# Discover and execute the project test runner across all packages containing environment configuration modules.
# Discover and run the project type-checking and linting suites to ensure all environment variable usages reference the validated configuration object.
```

**Accept when:**
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>