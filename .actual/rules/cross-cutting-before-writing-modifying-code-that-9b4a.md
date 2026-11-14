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
- **R-OPN-002** MUST: Find the dependency manifest in the repo to declare ranges, identify the build tool, and inspect the repository lock or resolution artifact to determine the exact resolved version.
- **R-OPN-003** MUST: Look up official documentation, changelog, or public API reference for the exact resolved version without relying solely on training-data recall.
- **R-OPN-004** MUST: Confirm every API, class, or function called exists in that exact version's documentation before using it, and re-run inspection for version-sensitive behavior per dependency at the point of use.
- **R-OPN-005** MUST: Ensure diagnostic checker endpoints and monitor mutation procedures share identical assertion schema imports from `@openstatus/assertions` to preserve runtime payload parity.

### Verify

```bash
# Discover the workspace static analysis and type verification script from the repository manifest and execute it
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by `@openstatus/assertions` without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>