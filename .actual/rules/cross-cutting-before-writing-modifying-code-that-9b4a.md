---
version: 2
name: cross-cutting-before-writing-modifying-code-that-9b4a
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Before Writing Modifying Code That Depends"
metadata:
  actual:
    adr_id: 9b4ad103-e84d-40fa-adb5-7fcfd07daef5
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Before Writing Modifying Code That Depends

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Before writing or modifying code that depends on versioned workspace or external dependencies, the consumer MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked version.
- **R-OPN-002** MUST: Find the dependency manifest in the repo to identify the build tool and inspect the repository lock or resolution artifact to determine the exact resolved version before writing code that uses a versioned library.
- **R-OPN-003** MUST: Look up official documentation, changelog, or public API reference for the exact resolved version without relying solely on training-data recall.
- **R-OPN-004** MUST: Confirm every API, class, or function to be called exists in that exact version's documentation before using it.
- **R-OPN-005** MUST: Re-run the version inspection and lookup steps per dependency at point of use for version-sensitive behavior.
- **R-OPN-006** MUST: Utilize schemas exported by `@openstatus/assertions` for all API endpoints accepting assertion inputs, ensuring diagnostic checker endpoints and monitor mutation procedures share identical assertion schema imports.
- **R-OPN-007** MUST: Register new assertion types within `@openstatus/assertions` and update the discriminated union export before referencing them in router input schemas.

### Verify

```bash
# Discover the workspace static analysis and type verification script from the repository manifest and execute it
# Example command discovery loop:
# 1. Inspect package.json or project manifest for type-check / workspace build scripts
# 2. Run type checking and tests
npm run check
npm test
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration type checks and code reviews enforcing no ad-hoc assertion schemas are defined in router layers.
</enforcement>