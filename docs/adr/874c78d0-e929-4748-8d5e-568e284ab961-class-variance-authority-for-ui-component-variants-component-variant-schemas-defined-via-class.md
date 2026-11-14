---
version: 2
name: 874c78d0-e929-4748-8d5e-568e284ab961-class-variance-authority-for-ui-component-variants-component-variant-schemas-defined-via-class
description: "class-variance-authority for UI Component Variants: Component Variant Schemas Defined Via Class"
metadata:
  actual:
    adr_id: 874c78d0-e929-4748-8d5e-568e284ab961
    status: accepted
    candidate_group_id: 62b6fa39-2353-401c-8aa0-215823a99517
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# class-variance-authority for UI Component Variants: Component Variant Schemas Defined Via Class

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/ui/**`

## Context

- Presentation components in the shared UI package require distinct visual permutations, including intent, size, and interaction states.
- Managing conditional class lists through ad-hoc string concatenation or manual branching introduces divergence and degrades maintainability across component boundaries.
- Static analysis of the UI component library detects uniform dependency on class-variance-authority alongside shared utility functions across atomic and composite component definitions.

## Problem Statement

Without a standardized mechanism for declaring component visual states, styling logic fragments across individual component files, leading to inconsistent prop naming, uncoordinated modifier precedence, and untyped variant declarations that increase maintenance overhead.

## Decision

1. MUST: Component variant schemas defined via class-variance-authority MUST export their derived variant property types to ensure type safety across consuming modules.

## Policy Block

- MUST Component variant schemas defined via class-variance-authority MUST export their derived variant property types to ensure type safety across consuming modules.

In scope:
- User interface components authoring visual variants, intents, or size modifiers within the design system package

Out of scope:
- Non-visual utility functions, headless logic containers, and backend service modules

## Rationale

- Centralizing visual variants through class-variance-authority establishes predictable component property interfaces and prevents styling divergence across design system elements.
- Static typing derived directly from variant schemas eliminates runtime mismatches between declared component properties and rendered class attributes.
- Observed implementation across atomic and composite UI modules demonstrates a shared, repeatable pattern that simplifies developer onboarding and component composition.

## Consequences

Positive:
- Uniform declaration of visual states across all design system components improves code readability and discoverability.
- Exported variant prop types provide compile-time guarantees that consuming templates supply valid visual configurations.
- Compound variant support simplifies the expression of interdependent visual modifier states.

Negative:
- Direct dependency on an external variant compilation utility couples component styling declarations to its schema syntax.
- Complex multi-variant schemas with nested compound configurations can increase declaration verbosity and bundle footprint.

## Alternatives

- Ad-hoc conditional string concatenation and manual branching (rejected)
  Rejected because: Manual conditional logic lacks structural validation, produces repetitive styling conditionals, and fails to generate automated type definitions.
  When valid: Trivial single-element components with no visual modifiers or discrete states.
- Custom style dictionary lookups and raw selector mapping (rejected)
  Rejected because: Custom mapping dictionaries require duplicate maintenance of selector names and interface types without standardized compound variant coordination.
  When valid: Environments where utility-first styling abstractions are prohibited or incompatible with build constraints.

## Risks

- Inconsistent default variant definitions between related components leading to visual misalignment.
  Mitigation: Enforce standard default variant declarations in all component schemas and verify consistency via automated design system test suites.
  Owner: Frontend engineering team
- Breaking API changes across library versions causing schema compilation failures.
  Mitigation: Adhere to the Discovery Policy lock-version grounding protocol to ensure all API calls match the exact resolved library version.
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
- When authoring a component variant schema, define the baseline styles first, followed by specific variant keys such as intent and size, and finally any compound variant overrides.
- Export the derived variant property type alongside the component declaration so consumers can type-check variant props without manual interface re-declaration.

## Continuation Context


Verify commands:
- Discover and run the project's type-checking script from the repository manifest to verify variant prop type compliance.
- Discover and execute the project's linter and component test runners to ensure variant schemas conform to style guidelines.

Accept when:
- Type checking passes without errors across all component definitions importing class-variance-authority.
- All component test suites validate that declared variants render their corresponding styling configurations.

## Enforcement

- Verified by: Continuous integration type-checking and automated test verification
- Verified by: Peer code review for all new and modified presentation components
- Violation handling: Pull requests containing untyped or ad-hoc conditional component styling variants are blocked during automated checks and code review.
- Exception process: Exceptions require documented approval from the design system architecture team detailing why external variant orchestration cannot be applied.