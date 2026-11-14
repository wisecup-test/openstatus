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

These rules are ALWAYS ACTIVE for all backend service domain handlers and modules residing within the services layer, and cross-cutting helper functions and generic computational algorithms intended for reuse across multiple service boundaries.

### Rules

- **R-UTIL-001** SHOULD: Utility logic placed in `@openstatus/utils` SHOULD remain domain-agnostic, stateless, and side-effect-free.
- **R-UTIL-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-UTIL-003** MANDATORY: Before writing code that uses a versioned library, the consumer MUST execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version's documentation, and re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-UTIL-004** MANDATORY: When adding functions to the shared utility library, ensure inputs and outputs use generic primitives rather than domain-specific entity models.
- **R-UTIL-005** MANDATORY: Organize utility functions into cohesion-focused submodules to allow tree-shaking and avoid unnecessary transitive imports in consuming services.
- **R-UTIL-006** MANDATORY: Enforce strict code review and automated linting boundaries preventing domain entities or service-level packages from being imported into the shared utility module.
- **R-UTIL-007** MANDATORY: Require unit test coverage for all exported utility functions and adhere to semver-compatible internal refactoring practices.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify compliance with internal module import boundaries.
# Discover and run the project test suites covering both the shared utility module and dependent service handlers.
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>