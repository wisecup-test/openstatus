---
version: 2
name: c419fab2-b020-429f-b251-80b8205c6f4d-adoption-of-internal-utils-module-for-shared-core-utilities-internal-utility-module-exports-not-introduce
description: "Adoption of Internal utils Module for Shared Core Utilities: Internal Utility Module Exports Not Introduce"
metadata:
  actual:
    adr_id: c419fab2-b020-429f-b251-80b8205c6f4d
    status: accepted
    candidate_group_id: 2606674d-2b12-4407-97f5-201fea1d93af
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
---

# Adoption of Internal utils Module for Shared Core Utilities: Internal Utility Module Exports Not Introduce

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/**`

## Context

- Multiple API endpoints and server-side visual generation components require access to shared helper routines, formatting primitives, and cross-cutting utility functions.
- Absence of a standardized utility module leads to fragmented, duplicate helper implementations across separate route subtrees and component hierarchies.
- Centralizing common helper functions within a shared internal utility module provides a single authoritative implementation across application route handlers and visual rendering layers.

## Problem Statement

When developing independent API route handlers and server-rendered generation components, cross-cutting requirements such as value transformation, string formatting, and class composition frequently arise. Without an architectural convention mandating the consolidation of these functions in a shared internal utility module, developers duplicate similar logic locally across route directories, leading to divergent implementations, inconsistent error handling, and maintenance overhead.

## Decision

1. MUST_NOT: Internal utility module exports MUST_NOT introduce side effects, maintain mutable module-level state, or perform global runtime modifications.

## Policy Block

- MUST_NOT Internal utility module exports MUST_NOT introduce side effects, maintain mutable module-level state, or perform global runtime modifications.

In scope:
- All API route handlers, server-rendered components, and visual generation subtrees requiring cross-cutting utility functions.
- Shared presentation and layout components requiring common transformation or formatting helpers.

Out of scope:
- Domain-specific services, data access layer queries, and specialized business logic modules.
- Independent isolated microservices or packages with their own dedicated utility modules.

Exceptions:
- EXC-20-001: A route handler or component requires specialized performance optimizations that cannot be generalized

## Rationale

- Evidence across multiple route handlers and visual generation components demonstrates consistent reliance on a centralized utils internal module for cross-cutting operations.
- Standardizing on a single internal module eliminates duplicate utility code, improves maintainability, and prevents divergent implementations across route boundaries.
- Keeping cross-cutting helper functions centralized and pure simplifies unit testing and facilitates safe reuse across both synchronous and asynchronous execution contexts.

## Consequences

Positive:
- Eliminates duplicate helper function implementations across route handlers and rendering components.
- Provides a single authoritative source for common data transformations and styling utilities.
- Improves testability by centralizing pure utility functions into isolated, reusable units.

Negative:
- Creates a centralized dependency where breaking changes to utility functions can impact multiple route handlers and rendering components.
- Risks utility module bloat if domain-specific logic is improperly placed into the shared module.

## Alternatives

- Localized Ad-Hoc Utilities in Each Route Directory (rejected)
  Rejected because: Duplicating utility functions locally across route directories creates divergent behaviors, increases codebase size, and compounds maintenance effort.
  When valid: Valid only in completely decoupled, independently deployed standalone micro-frontends with no shared runtime.
- Publishing Utilities as an External Third-Party Package (rejected)
  Rejected because: Publishing to an external registry introduces publishing latency, version management overhead, and unnecessary build pipeline complexity for application-internal helpers.
  When valid: Valid when utilities must be shared across disparate organizational repositories outside the application codebase.

## Risks

- Accumulation of unrelated domain-specific logic turning the utility module into an unmaintainable grab-bag.
  Mitigation: Enforce strict code review boundaries and automated structural linting to reject domain-specific logic from entering the core utility module.
  Owner: Engineering Team
- Accidental introduction of server-only or client-only side effects causing runtime failures in mixed execution environments.
  Mitigation: Mandate that all utility module exports remain pure, stateless functions with automated test suites verifying side-effect-free execution.
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
- When authoring or modifying shared helper functions in the internal utility module, developers must ensure all exported routines remain pure and free from side effects to maintain safe reusability across both client components and server route handlers.
- Domain-specific utility logic must reside within corresponding domain or feature directories rather than accumulating in the shared core utility module.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify compliance with internal module import conventions.
- Discover and execute the project unit test suite targeting the shared utility module to verify pure function contracts.

Accept when:
- All API routes and rendering components import shared utilities exclusively from the centralized internal utils module with zero linting violations.
- All test suites for the shared utility module pass completely without errors.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository linting and static analysis checks.
- Verified by: Peer code review mandatory for all changes modifying or adding exports to the shared utility module.
- Violation handling: Pull requests introducing duplicate utility logic or importing unauthorized localized copies are blocked until refactored to use the shared module.
- Exception process: Developers must submit an architectural exception proposal approved by the technical lead detailing why localized utilities are strictly necessary.