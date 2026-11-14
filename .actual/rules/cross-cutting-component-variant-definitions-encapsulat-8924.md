---
version: 2
name: cross-cutting-component-variant-definitions-encapsulat-8924
description: "Adoption of class-variance-authority for Component Variant Styling: Component Variant Definitions Encapsulated Designated Schemas"
metadata:
  actual:
    adr_id: 8924e0cc-2633-42cd-9fee-174308ce21ff
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Component Variant Definitions Encapsulated Designated Schemas

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all presentation components that expose configurable visual variants, intents, or sizes, and shared design system primitives and form elements requiring multi-state styling contracts.

### Rules

- **R-CVA-001** MUST: Component variant definitions MUST be encapsulated in designated variant schemas rather than constructed through inline conditional string concatenation.
- **R-CVA-002** DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-CVA-003** LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-CVA-004** Construct component variant schemas outside the render lifecycle to prevent redundant schema initialization.
- **R-CVA-005** Compose variant schema output with shared class-merging utilities to allow safe downstream class overrides.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute the component test suite to validate variant rendering.
# Discover the static analysis script from the project manifest and run it to verify that component styling conforms to standard variant declarations without unmanaged class string concatenation.
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>