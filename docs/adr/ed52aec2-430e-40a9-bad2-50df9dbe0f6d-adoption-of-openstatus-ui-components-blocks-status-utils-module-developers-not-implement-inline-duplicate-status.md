---
version: 2
name: ed52aec2-430e-40a9-bad2-50df9dbe0f6d-adoption-of-openstatus-ui-components-blocks-status-utils-module-developers-not-implement-inline-duplicate-status
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Not Implement Inline Duplicate Status"
metadata:
  actual:
    adr_id: ed52aec2-430e-40a9-bad2-50df9dbe0f6d
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Not Implement Inline Duplicate Status

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- Multiple client-rendered UI block components render status indicators, status timelines, and status events across the user interface.
- Presentation components require consistent status formatting, calculation, and transformation routines to avoid presentation divergence.
- Static analysis reveals concurrent adoption of @openstatus/ui/components/blocks/status.utils across status presentation components to centralize domain logic.

## Problem Statement

Distributing status evaluation and formatting logic across individual status presentation components causes inconsistent status calculations, redundant test implementations, and maintenance overhead when domain rules change.

## Decision

1. MUST_NOT: Developers MUST NOT implement inline or duplicate status calculation or formatting functions within individual status block components.

## Policy Block

- MUST_NOT Developers MUST NOT implement inline or duplicate status calculation or formatting functions within individual status block components.

In scope:
- Status visualization and event presentation components requiring status formatting, calculation, or shared transformation helpers
- UI block components rendering status indicators, events, or metric timelines

Out of scope:
- Generic presentation primitives and design system elements without status domain logic
- Server-only data fetching modules that do not perform client UI presentation formatting

## Rationale

- Centralizing status calculation and formatting in @openstatus/ui/components/blocks/status.utils eliminates duplicated logic across distinct presentation blocks such as events, timelines, and localization wrappers.
- Extracting status helpers into a dedicated internal utility module isolates pure transformation functions from client component lifecycle hooks and design system primitives.
- Consolidated utility functions ensure consistent status evaluation rules across all interactive visual blocks in the UI package.

## Consequences

Positive:
- Guarantees consistent status calculations and status presentation across all visual status blocks.
- Simplifies unit testing by decoupling deterministic status utility logic from component lifecycles.
- Reduces bundle duplication by sharing common helper routines across multiple UI block components.

Negative:
- Couples status presentation components to the public interface of the shared utility module.
- Requires updating the shared module and regression testing all dependent block components when utility function signatures evolve.

## Alternatives

- Inline helper functions defined locally within each status block component (rejected)
  Rejected because: Duplicating status computation logic across components leads to synchronization drift, inconsistent status representations, and redundant test coverage.
  When valid: Valid only for one-off component prototypes with no shared domain requirements.
- Distributing utilities across individual component subdirectories rather than a centralized module (rejected)
  Rejected because: Fragmented utility locations complicate cross-component sharing and obscure the canonical source of status calculation logic.
  When valid: Valid when components have zero overlapping domain calculations or shared models.

## Risks

- Accidental addition of stateful or DOM-dependent code into the shared utility module causing client component hydration or rendering issues
  Mitigation: Enforce that all functions in the utility module remain pure and decoupled from component state or browser APIs.
  Owner: Frontend engineering team
- Breaking changes in shared utility function signatures impacting multiple status visualizers simultaneously
  Mitigation: Enforce strict static type checking and comprehensive regression test suites across all consuming UI blocks prior to merging utility modifications.
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
- Discover the workspace module resolution configuration to ensure path aliases for the shared utility module resolve consistently across all consuming components.
- Maintain pure functional signatures for all exports from the shared status utilities module to facilitate isolated unit verification.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis and linting command from the root manifest to validate module import paths and boundary rules.
- Discover and run the project test suite command to validate the correctness of shared status utility calculations across consuming components.
- Discover and run the project type checker command to verify interface compatibility between the utility module and consuming blocks.

Accept when:
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

## Enforcement

- Verified by: Automated continuous integration checks executing repository linting, type validation, and unit tests on every pull request.
- Verified by: Peer code reviews verifying that status logic is imported from the canonical utility module rather than reimplemented locally.
- Violation handling: Continuous integration blocks pull requests containing local duplication of status calculation helpers or unresolved module imports.
- Violation handling: Code review requires refactoring any inline status helper logic into the centralized status utility module.
- Exception process: Exceptions require documented architectural review and approval from the frontend engineering team explaining why component-specific logic cannot reside in the shared utility module.