---
version: 2
name: cross-cutting-procedures-exposing-public-data-projecti-d121
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Procedures Exposing Public Data Projections Declare"
metadata:
  actual:
    adr_id: d121cd34-221d-4069-99cb-edc443a1fa7c
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Procedures Exposing Public Data Projections Declare

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all public and protected procedure definitions accepting parameters from external callers and runtime parsing of dynamic configuration objects and deserialized entity attributes.

### Rules

- **R-ZOD-001** SHOULD: Procedures exposing public data projections SHOULD declare explicit output validation schemas to prevent unintentional data leakage from internal models.
- **R-ZOD-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version.
  4. Look up the official documentation, changelog, or public API reference for that exact version.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-ZOD-004** MANDATORY: Define reusable schema fragments for shared domain identifiers, pagination cursors, and enumeration values to prevent validation drift across routers.
- **R-ZOD-005** MANDATORY: Apply boundary normalizations such as lowercase casing transforms and number coercions directly within schema declarations before passing parsed inputs to downstream services.

### Verify

```bash
# Discover the repository build and type verification script from the project manifest and execute it to confirm schema compilation across all procedure definitions.
# Discover and execute the repository automated test runner to validate that malformed input payloads are rejected and valid payloads are accepted.
# Discover and run the project static analysis linter to ensure all procedure definitions declare input validation contracts without bypasses.
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration will fail upon unvalidated input parameters or missing procedure schemas.
</enforcement>