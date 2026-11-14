---
version: 2
name: cross-cutting-openstatus-utils-module-not-import-4621
description: "Adoption of @openstatus/utils Core Utility Library: Openstatus Utils Module Not Import Service"
metadata:
  actual:
    adr_id: 46211c3d-bde9-4ec0-873f-745a86a912c6
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

# Adoption of @openstatus/utils Core Utility Library: Openstatus Utils Module Not Import Service

Scope: applies to `packages/services/src/monitor/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST_NOT: The `@openstatus/utils` module MUST NOT import service domain handlers, database schemas, or service-specific business logic.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts
# Discover and run the project test suites covering both the shared utility module and dependent service handlers
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated repository linting, static analysis checks, and code review validate module import paths.
</enforcement>