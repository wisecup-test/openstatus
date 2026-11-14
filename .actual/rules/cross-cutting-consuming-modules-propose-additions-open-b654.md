---
version: 2
name: cross-cutting-consuming-modules-propose-additions-open-b654
description: "Adoption of @openstatus/utils Shared Utility Library: Consuming Modules Propose Additions Openstatus Utils"
metadata:
  actual:
    adr_id: b654c0c7-c34d-4505-96b7-c20f2abdea1c
    status: accepted
    candidate_group_id: 7e427145-5612-4048-851d-1bafe1b94f4d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/**
---

# Adoption of @openstatus/utils Shared Utility Library: Consuming Modules Propose Additions Openstatus Utils

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application routes, service layers, and shared domain libraries requiring general utility, sanitization, or normalization helpers.

### Rules

- **R-UTIL-001** MAY: Consuming modules MAY propose additions to @openstatus/utils when a helper function is determined to be non-domain-specific and needed across multiple features.
- **R-UTIL-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the repository.
- **R-UTIL-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, and confirm every API/class/function exists in that version's documentation.
- **R-UTIL-004** MANDATORY: Inspect internal workspace package export definitions to discover all available utility signatures and contracts before introducing new helpers.
- **R-UTIL-005** MANDATORY: Ensure new utility routines added to @openstatus/utils are accompanied by isolated unit tests verifying edge cases and deterministic output.

### Verify

```bash
# Discover the repository static analysis command from the root workspace configuration and execute it to verify module import boundaries.
# Discover the repository test execution command and run the test suite covering shared utility imports and consuming components.
```

**Accept when:**
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated static analysis checks, pull request reviews, and CI blocking of unapproved duplicate utility functions.
</enforcement>