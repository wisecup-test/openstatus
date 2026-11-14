---
version: 2
name: 7a1e91c4-c2c6-4457-8367-f97d2d89a46c-adoption-of-lib-utils-internal-utility-module-for-presentation-components-developers-inspect-repository-lock-artifact-verify
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: Developers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 7a1e91c4-c2c6-4457-8367-f97d2d89a46c
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: Developers Inspect Repository Lock Artifact Verify

Status: proposed
Date: 2025-05-19
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- Multiple user interface components across data tables, layouts, and interactive elements require shared helper logic for presentation formatting and style manipulation.
- Decentralized or duplicated utility helper functions create maintenance drift and inconsistent presentation behavior across user interface views.
- The codebase establishes a centralized internal module pattern via the @/lib/utils module to provide common operations across the presentation layer.

## Problem Statement

Without a unified internal utility module, presentation components duplicate formatting and style composition logic across disparate component directories. This fragmentation leads to inconsistent user interface behavior, increases bundle overhead through redundant implementations, and complicates maintenance when shared behavior requires modification.

## Decision

1. MUST: Developers MUST inspect the repository lock artifact to verify the resolved version of any third-party dependency supporting utility routines before invoking or expanding module exports.

## Policy Block

- MUST Developers MUST inspect the repository lock artifact to verify the resolved version of any third-party dependency supporting utility routines before invoking or expanding module exports.

In scope:
- Presentation tier components including data table cells, layout containers, and user interface elements that share utility logic across the application.

Out of scope:
- Standalone backend services, isolated database schema definitions, and server-side worker tasks that do not render user interface components.

## Rationale

- Evidence across twenty-four component modules demonstrates consistent reliance on @/lib/utils for presentation helpers and formatting routines.
- Consolidating shared utility helpers into a single internal module provides a predictable contract for presentation components and simplifies testing.
- Standardizing on @/lib/utils prevents implementation divergence across data table cells, layout containers, and interactive widgets.

## Consequences

Positive:
- Eliminates duplicate helper implementations across presentation components and data table views.
- Establishes a single discoverable location for shared user interface utility functions.
- Ensures consistent behavior for class name resolution and presentation formatting throughout the dashboard.

Negative:
- Creates a centralized dependency where breaking changes in utility functions can impact multiple presentation components simultaneously.
- Risks module bloat if developers place domain-specific or unshared logic into the core utility module without discipline.

## Alternatives

- Component-local inline helper functions (rejected)
  Rejected because: Causes widespread code duplication, maintenance drift, and inconsistent behavior across user interface components
  When valid: When a helper function is strictly specific to a single component and has no utility elsewhere
- Decentralized domain-specific utility modules distributed across subdirectories (rejected)
  Rejected because: Creates discovery challenges and leads to overlapping utility implementations across domains
  When valid: When utilities are tightly coupled to a dedicated domain boundary with no shared presentation requirements

## Risks

- Uncontrolled growth and module bloat in the central utility module as miscellaneous helper routines accumulate.
  Mitigation: Enforce strict code review standards requiring utilities to be genuinely shared, pure, and general-purpose before inclusion.
  Owner: Frontend engineering team
- Accidental introduction of regressions across multiple components when modifying shared utility functions.
  Mitigation: Require thorough automated test coverage for all exported functions in the utility module before merging changes.
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
- Ensure all exported functions within the internal utility module are pure functions without global state or side effects.
- Structure utility functions with explicit TypeScript type signatures and parameter validation to prevent runtime errors in consuming components.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script to ensure all consuming components properly import and type-check against the utility module.
- Discover and execute the repository test suite to verify that utility functions and their consuming presentation components satisfy all test assertions.
- Discover and execute the repository linter to confirm that imports adhere to internal module path-aliasing conventions.

Accept when:
- Type checking completes with zero diagnostic errors across all components importing the utility module.
- All automated unit and regression tests for utility helpers and consuming components pass without failures.
- Linting checks confirm no duplicate local utility implementations or forbidden relative paths exist.

## Enforcement

- Verified by: Automated continuous integration validation pipelines executing type checking, linting, and test suites on pull requests.
- Verified by: Peer code review verifying that shared helper logic is routed through the central utility module rather than duplicated locally.
- Violation handling: Pull requests containing duplicated utility logic or violating module path boundaries must be blocked from merging until corrected.
- Exception process: Exceptions for one-off isolated component helpers require explicit documentation in the pull request and sign-off from frontend architectural leads.