---
version: 2
name: c04dcfd4-1bd8-4e05-b40f-20941a29788e-adoption-of-class-variance-authority-for-component-variant-styling-variant-declarations-use-cva-function-declare
description: "Adoption of class-variance-authority for Component Variant Styling: Variant Declarations Use Cva Function Declare"
metadata:
  actual:
    adr_id: c04dcfd4-1bd8-4e05-b40f-20941a29788e
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Variant Declarations Use Cva Function Declare

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- Multiple user interface presentation components require consistent styling across diverse visual states, color intents, and structural configurations.
- Without a unified variant abstraction, conditional class names proliferate into imperative string interpolations that lack static typing and uniform interface contracts.
- The codebase adopts class-variance-authority across presentation components to standardize variant schema definitions and runtime class resolution.

## Problem Statement

UI components with multiple visual states, sizes, and layout permutations require a declarative and type-safe mechanism to define and resolve class name mappings without manual, error-prone conditional string composition.

## Decision

1. MUST: Variant declarations MUST use the cva function to declare base class names and variant mapping tables with explicit default variants.

## Policy Block

- MUST Variant declarations MUST use the cva function to declare base class names and variant mapping tables with explicit default variants.

In scope:
- Presentation and UI component declarations requiring dynamic styling variants or state-based styling.

Out of scope:
- Non-visual utility functions, backend services, and pure business logic modules.

## Rationale

- Adopting class-variance-authority establishes a declarative schema for component variants, centralizing style rules and eliminating fragmented conditional logic.
- The pattern couples variant configuration directly with static type definitions, ensuring that invalid prop combinations are flagged during compilation.
- Evidence across multiple presentation component modules demonstrates an established convention for variant management and utility-based class assembly.

## Consequences

Positive:
- Eliminates repetitive manual class name concatenation logic across component implementations.
- Enforces compile-time type safety for component variant properties.
- Standardizes the component authoring interface across distinct presentation modules.

Negative:
- Introduces a structural dependency on an external library for component variant compilation.
- Requires engineering familiarity with the library configuration schema and type generation utilities.

## Alternatives

- Manual template literals and conditional string concatenation (rejected)
  Rejected because: Lacks compile-time property safety and creates fragmented, difficult-to-maintain class strings across components.
  When valid: Single-state components with static class names that have no configurable visual variants.
- CSS-in-JS runtime styling libraries (rejected)
  Rejected because: Adds client runtime overhead and conflicts with static utility-class based styling conventions.
  When valid: Applications requiring dynamic runtime theme value injection not expressible via utility class tokens.

## Risks

- Inconsistent default variant configurations could lead to unexpected visual rendering if not explicitly specified.
  Mitigation: Enforce explicit defaultVariants definitions within every cva schema declaration.
  Owner: Frontend engineering team
- Version drift or breaking API updates between dependency declarations and lock resolution artifacts.
  Mitigation: Follow the mandatory lock-version grounding process prior to library upgrades.
  Owner: Frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Declare cva definitions outside of component render functions to avoid re-allocating variant configuration objects on every render cycle.
- Extract composite variant interfaces using the VariantProps utility type to expose clean prop contracts to consuming components.

## Continuation Context


Verify commands:
- discover_repository_linter && execute_linter
- discover_repository_typechecker && execute_typecheck
- discover_repository_test_runner && execute_component_tests

Accept when:
- All component variant definitions compile without type diagnostics under the project typechecker.
- Component unit and visual regression tests pass without class resolution errors.

## Enforcement

- Verified by: Automated static analysis and typechecking during continuous integration.
- Verified by: Peer code review checking adherence to cva schema definitions for dynamic component props.
- Violation handling: Pull requests containing ad-hoc styling logic or manual string concatenation for variants are blocked until refactored into a cva schema.
- Exception process: Exceptions for one-off micro-components require review and documented approval from frontend architecture leads.