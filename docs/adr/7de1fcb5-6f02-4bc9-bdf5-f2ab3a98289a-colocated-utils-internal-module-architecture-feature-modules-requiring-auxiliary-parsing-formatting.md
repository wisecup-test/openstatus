---
version: 2
name: 7de1fcb5-6f02-4bc9-bdf5-f2ab3a98289a-colocated-utils-internal-module-architecture-feature-modules-requiring-auxiliary-parsing-formatting
description: "Colocated utils Internal Module Architecture: Feature Modules Requiring Auxiliary Parsing Formatting"
metadata:
  actual:
    adr_id: 7de1fcb5-6f02-4bc9-bdf5-f2ab3a98289a
    status: accepted
    candidate_group_id: 6a719b3d-fbf7-45b8-8a1e-ad0d8248fee7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
      - src/content/**
      - src/lib/checker/**
---

# Colocated utils Internal Module Architecture: Feature Modules Requiring Auxiliary Parsing Formatting

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/og/**`
- `src/content/**`
- `src/lib/checker/**`

## Context

- Feature implementations across the application require specialized transformations, schema validations, query filtering, and presentation helpers tailored to individual domain boundaries.
- Centralizing domain-specific helper logic into a monolithic application-wide helper namespace creates tight coupling across unrelated feature workflows and complicates refactoring.
- Colocating internal utils modules directly adjacent to consumers establishes modular ownership and clarifies dependency scope for route handlers, mock providers, and content processors.

## Problem Statement

Scattering feature-specific logic across disparate utility layers or concentrating localized helper functions into a global repository namespace introduces boundary degradation, cross-feature coupling, and high maintenance overhead when individual subsystem requirements change.

## Decision

1. SHOULD: Feature modules requiring auxiliary parsing, formatting, or mock generation logic SHOULD import directly from the local colocated utils module using relative path imports.

## Policy Block

- SHOULD Feature modules requiring auxiliary parsing, formatting, or mock generation logic SHOULD import directly from the local colocated utils module using relative path imports.

In scope:
- Feature-level modules, content transformation logic, mock providers, and endpoint handlers requiring scoped helper utilities.
- Internal domain directories managing localized data transformation, parsing, and formatting routines.

Out of scope:
- True cross-cutting enterprise primitives shared universally across all workspace packages.
- Framework runtime boundary files that do not encapsulate domain helper logic.

Exceptions:
- EXC-20-001: A utility function defined in a feature domain is formally promoted to a cross-cutting shared workspace package after adoption across multiple disparate domains.

## Rationale

- Colocating utils modules within each domain directory keeps auxiliary helper logic tightly bounded to the consumers that depend on it.
- Localizing utilities prevents the accumulation of dead code by ensuring that helper functions are deleted or updated alongside their corresponding feature implementations.
- Direct relative consumption of domain-specific helpers avoids premature abstraction and maintains clear architectural boundaries between distinct feature areas.

## Consequences

Positive:
- Improved domain cohesion and encapsulation by keeping helper logic directly adjacent to consuming workflows.
- Reduced risk of accidental cross-feature coupling and unintended side-effects from shared global helper mutations.
- Simplified lifecycle management and dead-code elimination when feature directories are removed or restructured.

Negative:
- Potential risk of localized logic duplication when similar transformation routines are implemented independently across different domains.
- Requires ongoing diligence during architectural reviews to identify opportunities where localized helpers should be elevated to shared workspace libraries.

## Alternatives

- Monolithic global utility package for all application helpers (rejected)
  Rejected because: Creating a monolithic global utility package leads to module bloat, obscures domain ownership, and causes unnecessary rebuilds across independent features.
  When valid: Small monolithic scripts where distinct domain boundaries do not exist.
- Inline helper logic directly within consumer handlers and components (rejected)
  Rejected because: Inlining transformation, validation, and parsing logic increases cyclomatic complexity and impairs testability within primary handler routines.
  When valid: Single-line trivial expressions that require neither reuse nor dedicated unit testing.

## Risks

- Divergent utility implementations across separate feature domains creating duplicate maintenance efforts.
  Mitigation: Conduct periodic cross-domain audits to identify shared helper patterns and elevate proven routines into workspace packages.
  Owner: Engineering Team
- Uncontrolled growth of colocated utils modules into complex multi-responsibility modules.
  Mitigation: Enforce module size limits and single-responsibility constraints during peer reviews.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Colocate helper functions in an internal utils module within the same directory as the consumers, keeping functions pure and focused on domain-specific transformations.
- Ensure that exports from colocated utils modules remain private to the enclosing feature directory unless explicitly approved for promotion to shared workspace scope.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and boundary verification script in the repository configuration and execute it to verify module isolation.
- Discover the repository test runner script and execute domain unit tests covering colocated utility modules.

Accept when:
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

## Enforcement

- Verified by: Automated structural import boundary checks in the continuous integration pipeline.
- Verified by: Peer code review verification during pull request evaluations.
- Violation handling: Continuous integration failure for unauthorized cross-feature relative imports.
- Violation handling: Rejection of pull requests introducing global utility pollution or unencapsulated domain helper functions.
- Exception process: Submit an architectural review request with justification to the Architecture Review Board for promotion of local utilities to shared workspace packages.