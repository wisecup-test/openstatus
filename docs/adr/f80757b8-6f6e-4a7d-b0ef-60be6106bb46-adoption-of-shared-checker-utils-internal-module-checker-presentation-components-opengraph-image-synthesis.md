---
version: 2
name: f80757b8-6f6e-4a7d-b0ef-60be6106bb46-adoption-of-shared-checker-utils-internal-module-checker-presentation-components-opengraph-image-synthesis
description: "Adoption of Shared checker/utils Internal Module: Checker Presentation Components Opengraph Image Synthesis"
metadata:
  actual:
    adr_id: f80757b8-6f6e-4a7d-b0ef-60be6106bb46
    status: accepted
    candidate_group_id: 6419eda7-589c-4d1a-b9ae-286e9010add2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of Shared checker/utils Internal Module: Checker Presentation Components Opengraph Image Synthesis

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`

## Context

- The system requires consistent checker calculations and formatting across user interface views and dynamic OpenGraph generation endpoints.
- Disparate implementations of checker domain logic across presentation and server routes increase the risk of behavioral divergence and drift.
- Centralizing checker operations into a dedicated domain utility module establishes a single source of truth for all checker-related operations.

## Problem Statement

Checker functionality is rendered across multiple application boundaries, including user-facing presentation routes and automated server-side image generation endpoints. Implementing domain operations independently across these boundaries introduces divergence in status calculations, formatting inconsistencies, and maintenance overhead.

## Decision

1. MUST: Checker presentation components and OpenGraph image synthesis endpoints MUST import shared checker domain logic exclusively from the shared checker/utils internal module rather than reimplementing domain transformations locally.

## Policy Block

- MUST Checker presentation components and OpenGraph image synthesis endpoints MUST import shared checker domain logic exclusively from the shared checker/utils internal module rather than reimplementing domain transformations locally.

In scope:
- All features, presentation components, and endpoints that process, format, or present checker domain data.

Out of scope:
- General-purpose utility functions unrelated to checker domain operations.

## Rationale

- Import analysis demonstrates that both user interface routes and server-side image generation routes consume the shared checker/utils module, confirming a shared architectural foundation for checker domain logic.
- Centralizing domain operations eliminates duplicate algorithmic implementations, guaranteeing behavioral parity between visual presentation and metadata generation.
- Decoupling shared utility logic into a dedicated module allows independent unit testing of core business transformations without mounting user interface trees.

## Consequences

Positive:
- Ensures consistency in checker calculations and formatting across client views and server endpoints.
- Reduces code duplication across presentation routes and image generation handlers.
- Simplifies regression testing by isolating domain algorithms into dedicated utility contracts.

Negative:
- Introduces shared coupling where modifications to utility contracts can impact both visual pages and server routes.
- Requires careful maintenance to prevent environment-specific dependencies from leaking into shared utilities.

## Alternatives

- Inlining domain calculations directly within each presentation component and endpoint (rejected)
  Rejected because: Inlining logic causes calculation drift, code duplication, and heightened testing complexity across endpoints.
  When valid: Valid only in single-use exploratory prototypes that will not reach production.
- Publishing domain logic as an external standalone package in a public registry (rejected)
  Rejected because: External package publication introduces release overhead and version management friction for application-specific domain utilities.
  When valid: Valid when the utility needs to be distributed across distinct repositories or external third-party consumers.

## Risks

- Leaking client-only or server-only dependencies into shared utility functions, breaking cross-environment compatibility.
  Mitigation: Enforce pure function signatures without environment globals or platform-specific imports.
  Owner: Engineering team
- Breaking changes in shared utility functions unintentionally corrupting OpenGraph image generation output.
  Mitigation: Implement automated regression testing and lint boundaries across both presentation and API layers.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the project dependency manifests and lock artifacts to ensure shared utility modules only depend on universally compatible helper libraries.
- Structure shared utility exports as granular functions to facilitate static analysis and optimize bundle delivery.

## Continuation Context


Verify commands:
- Discover and run the project test runner across all test suites covering shared domain utilities.
- Discover and execute the repository type checker and linter to confirm import boundaries and type consistency across modules.

Accept when:
- All unit tests covering shared domain utility functions pass with zero errors.
- Static type analysis and lint checks complete without type mismatches or boundary violations across presentation and API routes.

## Enforcement

- Verified by: Automated continuous integration checks covering unit tests, type analysis, and linting rules.
- Verified by: Mandatory peer code review for changes affecting shared utility modules.
- Violation handling: Continuous integration pipeline failures block pull request merging when import boundaries or tests fail.
- Violation handling: Code review rejection for duplicate inline domain logic in presentation or API routes.
- Exception process: Architectural exceptions require review and documented approval from the engineering team lead before merging.