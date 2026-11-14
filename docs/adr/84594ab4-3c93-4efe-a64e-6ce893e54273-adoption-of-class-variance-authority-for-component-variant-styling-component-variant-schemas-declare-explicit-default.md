---
version: 2
name: 84594ab4-3c93-4efe-a64e-6ce893e54273-adoption-of-class-variance-authority-for-component-variant-styling-component-variant-schemas-declare-explicit-default
description: "Adoption of class-variance-authority for Component Variant Styling: Component Variant Schemas Declare Explicit Default"
metadata:
  actual:
    adr_id: 84594ab4-3c93-4efe-a64e-6ce893e54273
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Component Variant Schemas Declare Explicit Default

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/**`

## Context

- User interface components across presentation domains require configurable visual variants to represent states, sizes, and intents.
- Ad-hoc class manipulation and manual ternary nesting create maintenance friction and inconsistent styling contracts across components.
- Multiple component implementations across content, forms, and common modules adopt class-variance-authority alongside shared interface utilities to manage styling permutations.

## Problem Statement

Managing component styling variants through manual string concatenation or scattered conditional expressions leads to fragile, difficult-to-maintain visual contracts and increases the risk of style collision or omission across diverse user interface components.

## Decision

1. SHOULD: Component variant schemas SHOULD declare explicit default variants for all defined variant dimensions.

## Policy Block

- SHOULD Component variant schemas SHOULD declare explicit default variants for all defined variant dimensions.

In scope:
- All presentation components that expose configurable visual variants, intents, or sizes.
- Shared design system primitives and form elements requiring multi-state styling contracts.

Out of scope:
- Components with completely static styling that never accept variant or intent properties.
- Non-visual logic components, hooks, or backend utility services.

## Rationale

- Consistently observed across component modules in forms, content cards, and navigation links.
- Centralizes variant definitions in declarative schemas, ensuring type safety and reducing visual regression bugs.
- Standardizes compound variant handling and default state fallbacks across the component library.

## Consequences

Positive:
- Provides structured, type-safe definitions for component visual states and variants.
- Encapsulates complex styling permutations and compound variants cleanly outside component render logic.
- Enforces consistent styling interfaces across disparate component modules.

Negative:
- Introduces an external dependency that must be maintained and tracked in lock resolution artifacts.
- Adds minor bundle footprint and abstraction overhead compared to primitive string interpolation.

## Alternatives

- Inline conditional string concatenation for component styling (rejected)
  Rejected because: Lacks type safety, obscures variant permutations, and scales poorly as component visual states multiply.
  When valid: Single-state components with static styling that require zero configurable visual permutations.
- Custom object-based variant lookup maps (rejected)
  Rejected because: Requires bespoke resolution logic for compound variants and duplicates typing boilerplate without standard library guarantees.
  When valid: Strict zero-dependency environments where third-party utility libraries are prohibited.

## Risks

- Over-engineering variant schemas for simple components leading to unnecessary boilerplate.
  Mitigation: Restrict variant schema creation to components with genuine visual permutations and review variant complexity during pull requests.
  Owner: engineering team
- Incompatibility between installed library version and utilized schema configuration APIs.
  Mitigation: Perform lock-version grounding prior to implementation and verify imported APIs against the resolved dependency documentation.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct component variant schemas outside the render lifecycle to prevent redundant schema initialization.
- Compose variant schema output with shared class-merging utilities to allow safe downstream class overrides.

## Continuation Context


Verify commands:
- Discover the repository test runner script from the project manifest and execute the component test suite to validate variant rendering.
- Discover the static analysis script from the project manifest and run it to verify that component styling conforms to standard variant declarations without unmanaged class string concatenation.

Accept when:
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and linting suites.
- Verified by: Peer review during pull requests evaluating component variant schema definitions.
- Violation handling: Pull requests introducing manual conditional class concatenation for multi-variant components will be blocked.
- Violation handling: Violations must be refactored to use standard class-variance-authority schemas before merging.
- Exception process: Submit an architectural exception request detailing why standard variant schema declarations cannot satisfy the specific styling requirement.
- Exception process: Obtain approval from the frontend engineering team before merging custom variant resolution logic.