---
version: 2
name: d1b2bf89-69a9-42a0-b026-8aed0ed1b010-adoption-of-openstatus-theme-store-for-theme-state-management-shared-theme-state-access-outside-layout
description: "Adoption of @openstatus/theme-store for Theme State Management: Shared Theme State Access Outside Layout"
metadata:
  actual:
    adr_id: d1b2bf89-69a9-42a0-b026-8aed0ed1b010
    status: accepted
    candidate_group_id: 084c8d7c-08dd-49c7-b2dd-833dba6a38cf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/theme-store for Theme State Management: Shared Theme State Access Outside Layout

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`

## Context

- The status-page application serves multiple distinct routing groups including domain-specific and public route trees.
- Application layouts across these routing groups require coordinated theme state presentation alongside ThemeProvider wrappers.
- Extracting theme state into a dedicated workspace module allows layouts to synchronize theme preferences without duplicating storage mechanisms.

## Problem Statement

Managing application theme state across disparate routing groups without a shared store causes visual flickering, inconsistent preference persistence, and duplicated state logic across layout components.

## Decision

1. SHOULD: Shared theme state access outside layout boundaries SHOULD be restricted to selectors and interfaces exported directly by @openstatus/theme-store.

## Policy Block

- SHOULD Shared theme state access outside layout boundaries SHOULD be restricted to selectors and interfaces exported directly by @openstatus/theme-store.

In scope:
- Top-level layout components managing application-wide visual presentation and theme configuration
- Route group layouts requiring synchronized theme persistence across nested views

Out of scope:
- Self-contained utility scripts or background workers that do not render UI elements
- Isolated third-party embed frames that do not share host application theme context

## Rationale

- Evidence across multiple layout files demonstrates consistent adoption of @openstatus/theme-store alongside ThemeProvider wrappers.
- A dedicated workspace module isolates theme storage and state mutation logic, allowing independent evolution of theme capabilities across consuming applications.
- Centralizing theme state prevents divergent client-side storage implementations across status page route groups.

## Consequences

Positive:
- Centralizes theme state management logic into a reusable workspace module across distinct application route groups
- Eliminates state divergence and synchronization issues between public and domain-specific status page layouts
- Decouples layout rendering components from state persistence and mutation logic

Negative:
- Introduces a cross-package dependency between application layouts and the shared workspace theme store module
- Requires changes to theme state contracts to be coordinated across workspace packages

## Alternatives

- Component-local React state and context providers within individual route layouts (rejected)
  Rejected because: Causes theme state fragmentation and redundant re-computation across sibling and nested route hierarchies
  When valid: Single-page prototypes with no shared layout persistence requirements
- Direct browser storage manipulation in individual components without a centralized store (rejected)
  Rejected because: Bypasses reactive state synchronization and creates tight coupling to client-side storage mechanisms
  When valid: Static non-interactive documents that do not support dynamic theme switching

## Risks

- Breaking API changes in @openstatus/theme-store could disrupt layout rendering across multiple application routes
  Mitigation: Enforce strict semantic versioning and workspace contract testing across consuming packages
  Owner: Frontend Engineering Team
- Mismatched theme store resolution across workspace packages resulting in multiple state instances
  Mitigation: Enforce unified workspace dependency resolution via project resolution artifacts
  Owner: Architecture Core Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Integrate ThemeProvider at the highest common layout level within route hierarchies to ensure uniform theme propagation.
- Expose theme values and updater actions exclusively through defined store interfaces to prevent hydration inconsistencies.

## Continuation Context


Verify commands:
- # Discover repository verification scripts from the project manifest and execute the test suite
- # Query the workspace dependency resolution artifact to verify that @openstatus/theme-store resolves correctly
- # Discover and execute the project linting and module boundary checks across layout components

Accept when:
- The workspace dependency resolution artifact confirms that @openstatus/theme-store is resolved to a single consistent version across all consuming layouts.
- Layout components successfully coordinate ThemeProvider with state derived from @openstatus/theme-store without introducing uncoordinated local theme state.
- All automated project validation scripts complete without reporting module boundary or theme state synchronization errors.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace static analysis and boundary verification
- Verified by: Peer code review verifying that theme state is imported exclusively from @openstatus/theme-store
- Violation handling: Pull requests introducing divergent theme state storage or bypassing @openstatus/theme-store are blocked from merging
- Violation handling: Existing uncoordinated theme implementations are flagged for refactoring into @openstatus/theme-store consumption
- Exception process: Developers must submit an architectural exception proposal detailing why an isolated theme context is required, subject to lead engineer approval.