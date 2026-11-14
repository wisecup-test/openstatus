---
version: 2
name: 98887cad-1a79-4542-95f5-415b465b2de7-adoption-of-date-fns-for-functional-date-and-time-manipulation-modules-performing-date-transformations-import-individual
description: "Adoption of date-fns for Functional Date and Time Manipulation: Modules Performing Date Transformations Import Individual"
metadata:
  actual:
    adr_id: 98887cad-1a79-4542-95f5-415b465b2de7
    status: accepted
    candidate_group_id: e914c52b-35bd-4db1-bc0f-89fb166ab748
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/status/**
      - src/lib/**
---

# Adoption of date-fns for Functional Date and Time Manipulation: Modules Performing Date Transformations Import Individual

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/og/status/**`
- `src/lib/**`

## Context

- Application utilities and API route handlers require consistent handling of timestamps, relative intervals, and date formatting.
- Direct manipulation of native temporal objects introduces mutable side effects and parsing inconsistencies across environments.
- Multiple modules across shared utilities and server endpoints import date-fns for specialized date calculations and formatting.

## Problem Statement

Handling temporal calculations, relative intervals, and date formatting using native primitives or disparate utilities leads to mutable side effects, inconsistent formatting, and code duplication across utility modules and API routes.

## Decision

1. MUST: Modules performing date transformations MUST import individual functional primitives directly from date-fns rather than implementing custom date arithmetic.

## Policy Block

- MUST Modules performing date transformations MUST import individual functional primitives directly from date-fns rather than implementing custom date arithmetic.

In scope:
- All application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

Out of scope:
- Low-level database schema timestamp definitions and storage engine native clock expressions.

## Rationale

- date-fns provides an immutable, functional approach where functions take standard date instances and return new instances without mutating arguments.
- Modular functional exports allow consumers to import only necessary operations, supporting minimal bundle overhead and clear dependency graphs.
- Evidence across monitoring helpers, shared utilities, and route handlers shows established reliance on date-fns for temporal operations.

## Consequences

Positive:
- Standardizes temporal arithmetic, interval comparisons, and formatting logic across utility and API boundaries.
- Ensures immutability by operating on pure date representations without mutating native objects.
- Enables optimal dead-code elimination through individual functional primitive imports.

Negative:
- Dependency footprint increases across shared utility layers and server endpoints.
- Upstream major version changes across functional date utilities require updates to import paths and function signatures.

## Alternatives

- Native Date object manipulation without external libraries (rejected)
  Rejected because: Native Date APIs suffer from mutable state, inconsistent cross-runtime parsing quirks, and cumbersome arithmetic logic.
  When valid: Simple timestamp retrieval or native ISO string serialization where no calculations or specialized formatting are required.
- Monolithic object-oriented date libraries (rejected)
  Rejected because: Monolithic date libraries bundle extensive unused locale and parsing logic into bundles and rely on mutable instance patterns.
  When valid: Legacy codebases already deeply coupled to object-oriented date wrapper chains.

## Risks

- Breaking API changes across major library releases altering function signatures or export structures.
  Mitigation: Verify exact resolved library versions via the lock artifact and consult version-specific documentation before calling functions.
  Owner: Engineering Team
- Inconsistent format string usage across different endpoints creating discordant presentation outputs.
  Mitigation: Encapsulate common formats and calculations in domain utilities so adjustments are localized to single utility definitions.
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
- Import only the specific functional utilities required for the operation to preserve modularity.
- Centralize recurring composite date operations, such as status window calculations and localized presentation formats, in shared utility modules.

## Continuation Context


Verify commands:
- Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it to confirm dependency resolution and module integrity.
- Inspect repository configuration to locate the project linting and static analysis commands, and execute them to ensure all date transformations comply with import standards.

Accept when:
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

## Enforcement

- Verified by: Automated linting rules prohibiting unapproved third-party date libraries.
- Verified by: Static type analysis and module boundary checks during project continuous integration.
- Verified by: Peer review of code changes involving temporal calculations and utility formatting.
- Violation handling: Pull requests introducing unapproved date libraries or direct mutable date arithmetic are flagged and blocked.
- Violation handling: Engineers must refactor temporal logic to use standard date-fns utility functions.
- Exception process: Submit an architectural review request detailing why date-fns primitives are insufficient for the specific temporal requirement.
- Exception process: Obtain sign-off from the technical lead prior to introducing any alternative temporal processing library.