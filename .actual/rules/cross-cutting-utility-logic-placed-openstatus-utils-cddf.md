---
version: 2
name: cross-cutting-utility-logic-placed-openstatus-utils-cddf
description: "Adoption of @openstatus/utils Core Utility Library: Utility Logic Placed Openstatus Utils Remain"
metadata:
  actual:
    adr_id: cddfbb87-7aec-4beb-a30b-68f283c8ee3e
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

# Adoption of @openstatus/utils Core Utility Library: Utility Logic Placed Openstatus Utils Remain

Scope: applies to `packages/services/src/monitor/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** SHOULD: Utility logic placed in `@openstatus/utils` MUST remain domain-agnostic, stateless, and side-effect-free.
- **R-UTIL-002** MANDATORY: The consumer MUST discover dependency management tools, lock files, and build commands directly from the repository.
- **R-UTIL-003** MANDATORY: Before writing code using a versioned library, execute the lock-version grounding process: find manifest, identify build tool, inspect lock/resolution artifact, look up official version-specific documentation, confirm APIs exist in that version, and re-verify per dependency at point of use.
- **R-UTIL-004** MANDATORY: When adding functions to the shared utility library, inputs and outputs MUST use generic primitives rather than domain-specific entity models.
- **R-UTIL-005** SHOULD: Organize utility functions into cohesion-focused submodules to allow tree-shaking and avoid unnecessary transitive imports in consuming services.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts
# Discover and run the project test suites covering both the shared utility module and dependent service handlers
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated repository linting, static analysis checks, and code reviews.
</enforcement>