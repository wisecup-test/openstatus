---
version: 2
name: cross-cutting-specialized-domain-error-modules-inherit-0e01
description: "Adoption of base-error Internal Module for Structured Error Definitions: Specialized Domain Error Modules Inherit Encapsulate"
metadata:
  actual:
    adr_id: 0e01a638-fbbf-4239-871a-ad54ad401307
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Specialized Domain Error Modules Inherit Encapsulate

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for specialized error classes and error definition modules within domain and transport packages, and error mapping adapters that transform external validation failures into internal domain errors.

### Rules

- **R-ERR-001** MUST: Specialized domain error modules MUST inherit from or encapsulate the internal base-error module to maintain a uniform error hierarchy and serialization contract.
- **R-ERR-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ERR-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation/changelog/public API reference for that exact version without relying solely on training-data recall. (5) Confirm every API, class, or function called exists in that exact version's documentation. (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-ERR-004** MUST: Construct all new specialized domain error types by extending the internal base-error abstraction and binding an error code from the internal error-code registry.
- **R-ERR-005** MUST: Route external schema validation errors through dedicated schema error adapters that normalize third-party validation issues into base error formats.

### Verify

```bash
# Discover the project test execution script from the workspace dependency manifest and execute test validation across error modules.
# Discover and execute the project type-checking and linting verification scripts to ensure compliance with base-error interface constraints.
```

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines executing static analysis and automated unit test suites on all pull requests, and peer code review. Pull requests introducing uncoordinated, standalone error definitions that bypass the base error module will be blocked until refactored into compliance.
</enforcement>