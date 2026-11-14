---
version: 2
name: 9f3afe7b-1fe0-44af-8d0b-e7223f46f94a-adoption-of-sonner-for-toast-notification-management-components-rendering-toast-container-hooks-triggering
description: "Adoption of sonner for Toast Notification Management: Components Rendering Toast Container Hooks Triggering"
metadata:
  actual:
    adr_id: 9f3afe7b-1fe0-44af-8d0b-e7223f46f94a
    status: accepted
    candidate_group_id: 9abf003d-098e-4232-bd99-6521c772f65f
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
      - src/hooks/**
---

# Adoption of sonner for Toast Notification Management: Components Rendering Toast Container Hooks Triggering

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/ui/**`
- `src/hooks/**`

## Context

- User interaction flows and asynchronous tasks require transient visual notifications to communicate operation results.
- Multiple user interface components and hooks within the shared UI workspace package trigger user feedback.
- Adopting a dedicated notification library eliminates divergent, ad-hoc notification implementations across packages.

## Problem Statement

Without a unified toast notification standard, frontend interaction hooks and UI components risk implementing divergent alert mechanisms, resulting in inconsistent user experiences and duplicative notification queue implementations.

## Decision

1. MUST: Components rendering the toast container and hooks triggering toast feedback MUST operate within client-rendered boundaries marked with the client component directive.

## Policy Block

- MUST Components rendering the toast container and hooks triggering toast feedback MUST operate within client-rendered boundaries marked with the client component directive.

In scope:
- User interface components and client interaction hooks requiring transient status feedback.
- Shared UI workspace modules managing visual alert presentation.

Out of scope:
- Server-side background services and non-visual utility functions.
- Persistent modal dialogs and inline validation error messages.

## Rationale

- Evidence demonstrates consistent adoption of sonner across UI components and interaction hooks within the shared UI package.
- Using a dedicated library centralizes notification queueing, stacking animations, and theme synchronization under an established client-side contract.
- Standardizing on sonner prevents duplicate notification state implementations across separate feature modules.

## Consequences

Positive:
- Provides a standardized, accessible toast notification interface across user-facing modules.
- Decouples notification display mechanics and stacking logic from individual consumer hook workflows.
- Coordinates toast appearance seamlessly with theme context providers.

Negative:
- Introduces an external dependency that must be tracked and kept compatible with UI framework versions.
- Requires consistent management of client execution directives across all consuming components and hooks.

## Alternatives

- Custom In-House Toast State Store (rejected)
  Rejected because: Creating a bespoke notification queue increases maintenance burden and risks synchronization issues across component trees.
  When valid: Valid when runtime environment constraints strictly prohibit external UI dependencies.
- Native Browser Dialogs (rejected)
  Rejected because: Native modal dialogs block thread execution and lack non-intrusive stacked toast presentation and theme coordination.
  When valid: Valid in primitive debugging environments or minimal fallback modes lacking graphical UI support.

## Risks

- Executing toast triggers in non-client rendering contexts causing runtime execution errors.
  Mitigation: Enforce client directive boundaries on toast containers and consuming hooks through static analysis checks.
  Owner: Frontend engineering team
- Dependency version drift or breaking changes affecting toast invocation signatures.
  Mitigation: Follow the mandatory lock-version grounding policy prior to dependency updates and verify toast calls against locked API specifications.
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
- Mount the main toast wrapper component within the top-level application layout tree to ensure toasts render across all view routes.
- Invoke toast notifications from interaction hooks using the standardized library dispatch functions.

## Continuation Context


Verify commands:
- Discover and run the repository test suite to validate toast notification rendering and interaction hook behavior.
- Discover and run the workspace static analysis and linting checks to ensure compliance with client boundary directives.

Accept when:
- All toast notification components render without runtime errors under client execution boundaries.
- Interactive hooks successfully dispatch toast notifications using sonner API bindings without regressions.
- Static analysis and test suites pass cleanly across all UI package modules.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace test and lint suites.
- Verified by: Peer code reviews evaluating client directive boundaries and toast dispatch patterns.
- Violation handling: Pull requests introducing unapproved toast libraries or redundant notification stores will be blocked.
- Violation handling: Improper invocation of toast APIs outside client component boundaries must be remediated prior to merge.
- Exception process: Submit an architectural waiver detailing why sonner cannot satisfy the notification requirement.
- Exception process: Obtain approval from the frontend architecture team before introducing any alternative notification library.