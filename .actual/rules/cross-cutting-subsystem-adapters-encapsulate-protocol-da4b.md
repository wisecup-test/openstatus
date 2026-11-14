---
version: 2
name: cross-cutting-subsystem-adapters-encapsulate-protocol-da4b
description: "Internal Domain Module Adoption: Subsystem Adapters Encapsulate Protocol Specific Headers"
metadata:
  actual:
    adr_id: da4be14f-5cc4-46e9-b2c7-310ccdcc3412
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Subsystem Adapters Encapsulate Protocol Specific Headers

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for application subsystems requiring domain entity operations, custom domain resolution, or authentication domain verification.

### Rules

- **R-DOM-001** SHOULD: Subsystem adapters SHOULD encapsulate protocol-specific headers, rewrites, and session states, delegating all domain state evaluations to the internal domain module.
- **R-DOM-002** MANDATORY: Consuming modules MUST treat the internal domain module as a stable boundary, consuming exported functions and domain entities rather than reaching into submodule internals.
- **R-DOM-003** MANDATORY: All new application features requiring domain resolution or entity updates MUST interface with the internal domain module instead of implementing ad-hoc validation.
- **R-DOM-004** MANDATORY: The consumer MUST discover and apply tool names, file names, commands, package managers, and version numbers directly from the project repository.
- **R-DOM-005** MANDATORY: Before writing code that uses a versioned library, the consumer MUST execute the lock-version grounding sequence: find dependency manifest, identify build tool, inspect lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute the repository static analysis suite to verify internal domain module import boundaries
# Discover and execute test runner suites validating proxy routing and authentication domain contracts
```

**Accept when:**
- All proxy routing and authentication modules resolve domain logic exclusively through the internal domain module without rule duplication.
- Discovered static analysis and test runner suites execute with zero errors across all domain consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations detected by static checks will block continuous integration pipelines until refactored, and pull requests introducing direct domain logic duplication or bypassing the internal domain module must be rejected during review.
</enforcement>