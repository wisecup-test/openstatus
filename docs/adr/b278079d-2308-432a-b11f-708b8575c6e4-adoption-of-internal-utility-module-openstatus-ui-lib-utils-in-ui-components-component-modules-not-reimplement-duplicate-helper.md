---
version: 2
name: b278079d-2308-432a-b11f-708b8575c6e4-adoption-of-internal-utility-module-openstatus-ui-lib-utils-in-ui-components-component-modules-not-reimplement-duplicate-helper
description: "Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Component Modules Not Reimplement Duplicate Helper"
metadata:
  actual:
    adr_id: b278079d-2308-432a-b11f-708b8575c6e4
    status: accepted
    candidate_group_id: 9ff50e4a-5fdd-4ce4-90ec-8cadf9e0c273
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
      - src/lib/**
---

# Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Component Modules Not Reimplement Duplicate Helper

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/**`
- `src/hooks/**`
- `src/lib/**`

## Context

- The user interface package contains dozens of shared components ranging from low-level primitive wrappers to complex status domain blocks.
- A majority of UI component modules consistently import helper utilities from a dedicated internal module (@openstatus/ui/lib/utils).
- Centralizing shared helper routines ensures uniform class composition, style resolution, and shared behavioral utilities across the design system.

## Problem Statement

UI components require consistent class name merging, style computation, and helper utilities. Inlining these helpers or directly importing external utilities across individual component files leads to duplicated logic, inconsistent styling behavior, and tight coupling across the component suite.

## Decision

1. MUST_NOT: Component modules MUST NOT reimplement duplicate helper routines or directly bypass the internal utility module when equivalent shared utilities exist.

## Policy Block

- MUST_NOT Component modules MUST NOT reimplement duplicate helper routines or directly bypass the internal utility module when equivalent shared utilities exist.

In scope:
- All component, block, and hook implementations located within the shared UI package.
- Shared formatting, styling, and general utility helper functions consumed by UI components.

Out of scope:
- Isolated standalone applications that do not consume the shared UI component package.
- Backend or server-only packages with no UI dependencies.

## Rationale

- Static analysis identifies 44 files importing the internal shared utility module across diverse UI components.
- A dedicated internal utility module establishes a single point of change for styling and helper routines throughout the component library.
- Standardizing on the internal module path eliminates divergent helper implementations across low-level and high-level components.

## Consequences

Positive:
- Centralizes common utility and class-name composition logic across all shared UI components.
- Maintains consistent design token and class resolution behavior across both low-level primitives and high-level status blocks.
- Decouples component implementations from underlying third-party class and style utility libraries.

Negative:
- Creates an internal dependency on the utility module across virtually all UI components, requiring changes to that module to maintain backward compatibility.
- Requires developers to conform to workspace-scoped import conventions rather than arbitrary relative file paths.

## Alternatives

- Inlining utility helper functions directly inside each component file (rejected)
  Rejected because: Causes code duplication across dozens of UI components and creates divergent formatting and styling logic across the component library.
  When valid: Valid for isolated, one-off single-file prototypes outside the shared UI component package.
- Directly referencing external utility libraries across all individual component modules without an internal abstraction layer (rejected)
  Rejected because: Tightly couples every component to third-party library signatures and prevents centralized extensions or adaptations.
  When valid: Valid when no project-wide styling adaptations or wrapper utilities are required.

## Risks

- Modifications to shared utility function signatures may introduce cascading regressions across consuming UI components.
  Mitigation: Enforce strict API stability and regression testing on the internal utility module to prevent breaking dependent UI components.
  Owner: engineering team
- Uncontrolled growth of the utility module leading to bloat and accumulation of domain-specific logic in shared space.
  Mitigation: Establish code review boundaries ensuring only generic, cross-cutting helpers are added to the utility module rather than domain-specific state logic.
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
- Ensure that all new component implementations inside the UI package import class composition and styling helpers exclusively from the designated internal utility module.
- Avoid exporting component-specific business logic from the shared utility module; keep it focused on cross-cutting helper functions.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting scripts to verify compliance with internal utility import paths.
- Discover and execute the package build and type verification scripts to confirm all utility module references resolve cleanly.

Accept when:
- All primitive and composite UI components in the shared UI package resolve utility logic via the internal shared library module.
- No direct imports of private or external utility implementations bypass the designated internal utility module.
- Automated static checks and build verification pass without unresolved workspace module references.

## Enforcement

- Verified by: Static analysis and import linter rules verifying that shared utility logic is consumed from the designated internal utility path.
- Verified by: Automated package build and type-check steps in continuous integration.
- Verified by: Peer code review for all new and modified UI component implementations.
- Violation handling: Pull requests containing duplicate helper routines or direct third-party utility bypasses must be blocked.
- Violation handling: Violations identified in automated checks must be refactored to consume the internal utility module before merging.
- Exception process: Submit an architecture review request detailing why the centralized internal utility module cannot accommodate the proposed helper logic.
- Exception process: Document temporary exceptions in code review with an associated remediation task.