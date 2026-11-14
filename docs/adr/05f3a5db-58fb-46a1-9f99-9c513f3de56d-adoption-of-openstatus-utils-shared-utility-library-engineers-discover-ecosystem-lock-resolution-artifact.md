---
version: 2
name: 05f3a5db-58fb-46a1-9f99-9c513f3de56d-adoption-of-openstatus-utils-shared-utility-library-engineers-discover-ecosystem-lock-resolution-artifact
description: "Adoption of @openstatus/utils Shared Utility Library: Engineers Discover Ecosystem Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 05f3a5db-58fb-46a1-9f99-9c513f3de56d
    status: accepted
    candidate_group_id: 7e427145-5612-4048-851d-1bafe1b94f4d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/**
---

# Adoption of @openstatus/utils Shared Utility Library: Engineers Discover Ecosystem Lock Resolution Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/lib/**`

## Context

- Application features including health check runners, API endpoints, and network guard filters require common utility helpers and sanitization functions.
- Independent feature modules previously risked duplicating common logic such as URL normalization, configuration inspection, and defensive validation.
- Static analysis reveals coordinated consumption of @openstatus/utils across API routes and backend library modules to standardize helper functionality.

## Problem Statement

Ad-hoc reimplementation of common helper routines across disparate API handlers and service modules causes behavioral divergence, redundant code maintenance, and uneven error handling across application boundaries.

## Decision

1. MUST: Engineers MUST discover the ecosystem lock resolution artifact and resolve the exact locked version of all internal workspace dependencies before authoring code that consumes shared utilities.

## Policy Block

- MUST Engineers MUST discover the ecosystem lock resolution artifact and resolve the exact locked version of all internal workspace dependencies before authoring code that consumes shared utilities.

In scope:
- All application routes, service layers, and shared domain libraries requiring general utility, sanitization, or normalization helpers.

Out of scope:
- Domain-specific business logic, database entities, and feature-scoped UI rendering routines.

## Rationale

- Consolidating shared utility logic within @openstatus/utils ensures uniform behavior for critical functions across all consuming features.
- Evidence demonstrates that core operational layers consistently depend on @openstatus/utils for helper capabilities alongside their respective domain responsibilities.
- A centralized utility module minimizes cognitive load for engineers by providing standard, tested building blocks for common operations.

## Consequences

Positive:
- Eliminates duplicate implementations of common routines across API endpoints and domain libraries.
- Establishes a single authoritative location for common URL normalization and cross-cutting utility helpers.
- Improves maintainability and auditability by isolating foundational helper logic away from feature business logic.

Negative:
- Changes to shared utility functions in @openstatus/utils can introduce breaking changes or unintended regressions across multiple consuming domains.
- Consuming modules take on a shared workspace dependency that requires coordinated publishing and dependency resolution.

## Alternatives

- Local in-file helper implementations within each route and service library (rejected)
  Rejected because: Leads to logic drift, duplicated edge-case handling, and inconsistent utility behavior across distinct application domains.
  When valid: Valid only for one-off prototyping where a module has no shared surface or common requirements with other features.
- Direct adoption of fragmented third-party micro-utility packages per feature module (rejected)
  Rejected because: Increases dependency bloat, introduces inconsistent API ergonomics, and complicates workspace-wide dependency updates.
  When valid: Valid when a highly specialized algorithmic function is required that falls outside the scope of internal shared utilities.

## Risks

- Unintended regressions in shared utility functions propagating to multiple application entry points.
  Mitigation: Maintain strict automated unit testing suites within the utility package covering all public helper functions before dependency version bumps.
  Owner: engineering team
- Accumulation of domain-specific business logic within the generic utility package leading to tight coupling.
  Mitigation: Enforce strict architectural review ensuring that domain-specific or stateful logic remains within service modules rather than entering the shared utility package.
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
- Inspect the internal workspace package export definitions to discover all available utility signatures and contracts before introducing new helpers.
- Ensure new utility routines added to @openstatus/utils are accompanied by isolated unit tests verifying edge cases and deterministic output.

## Continuation Context


Verify commands:
- Discover the repository static analysis command from the root workspace configuration and execute it to verify module import boundaries.
- Discover the repository test execution command and run the test suite covering shared utility imports and consuming components.

Accept when:
- Static analysis confirms that cross-cutting utility functions and URL helpers are imported from @openstatus/utils across target application modules.
- Automated test suites and type verification scripts pass without unresolved module imports or duplicated helper routines.

## Enforcement

- Verified by: Automated static analysis checks enforcing clean module boundary imports and dependency lint rules.
- Verified by: Peer code review verification on pull requests modifying or adding cross-cutting helper functions.
- Violation handling: Static analysis failures block continuous integration merges when unapproved duplicate utility functions are detected.
- Violation handling: Pull requests violating shared utility standards are rejected during automated checks and code reviews until refactored.
- Exception process: Engineers must submit an architectural exception request documenting why the shared utility package cannot support the required behavior.
- Exception process: Exceptions require approval from the lead maintainer and an explicit migration plan to upstream or decouple the custom utility logic.