---
version: 2
name: cross-cutting-consuming-service-handlers-import-only-7741
description: "Adoption of @openstatus/utils Core Utility Library: Consuming Service Handlers Import Only Specific"
metadata:
  actual:
    adr_id: 7741bbc2-d9c9-41f3-81ae-5b6411f9423d
    status: accepted
    candidate_group_id: 24f82739-f335-4572-9c13-bfa2a59d6c93
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
      - src/page-subscriber/**
---

# Adoption of @openstatus/utils Core Utility Library: Consuming Service Handlers Import Only Specific

Scope: applies to `packages/services/src/monitor/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** SHOULD: Consuming service handlers SHOULD import only the specific utility members required for their operational scope.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts
# Discover and run the project test suites covering both the shared utility module and dependent service handlers
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated repository linting, static analysis checks, and code review.
</enforcement>