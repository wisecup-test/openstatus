---
version: 2
name: cross-cutting-templates-encapsulate-common-structural-3ca9
description: "Adoption of react-email for Transactional Email Composition: Templates Encapsulate Common Structural Wrappers Composing"
metadata:
  actual:
    adr_id: 3ca94a30-f8f7-4be5-9b0b-15a210c63865
    status: accepted
    candidate_group_id: b6d4a005-7d3f-4d2d-974c-51b7961ef7bc
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - emails/**
      - src/**
---

# Adoption of react-email for Transactional Email Composition: Templates Encapsulate Common Structural Wrappers Composing

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for authoring, structuring, and maintaining transactional email templates, and rendering component trees into email markup for outbound notification services.

### Rules

- **R-TRE-001** MUST: Templates MUST encapsulate common structural wrappers by composing shared layout and styling components rather than duplicating container markup.
- **R-TRE-002** DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-TRE-003** LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-TRE-004** MUST: Structure each email notification template as a modular component consuming strongly typed property contracts.
- **R-TRE-005** MUST: Centralize visual tokens, container sizing, and typography primitives in shared styling modules to ensure brand uniformity across templates.

### Verify

```bash
# Discover and run the project type checking script from the dependency manifest.
# Discover and run the project test script to validate template rendering and schema integrity.
# Discover and run the project linter to ensure component styling and structure conform to repository standards.
```

**Accept when:**
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines run automated type verification, linting, and template test suites. Violations must be refactored to consume approved component primitives before pull requests can be merged.
</enforcement>