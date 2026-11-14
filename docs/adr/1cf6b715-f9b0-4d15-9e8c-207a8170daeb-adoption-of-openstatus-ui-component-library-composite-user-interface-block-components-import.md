---
version: 2
name: 1cf6b715-f9b0-4d15-9e8c-207a8170daeb-adoption-of-openstatus-ui-component-library-composite-user-interface-block-components-import
description: "Adoption of @openstatus/ui Component Library: Composite User Interface Block Components Import"
metadata:
  actual:
    adr_id: 1cf6b715-f9b0-4d15-9e8c-207a8170daeb
    status: accepted
    candidate_group_id: 9ee8de6e-eefa-477b-b480-83f4f7bb50f2
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui Component Library: Composite User Interface Block Components Import

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- Composite interface block components require standard interactive elements such as buttons, dropdown menus, and loading skeletons.
- Multiple client-rendered switcher components consistently import foundational primitives and shared utilities from the internal @openstatus/ui workspace module.
- Standardizing on a central component library avoids duplicated styling logic and divergent user experiences across application surfaces.

## Problem Statement

Composite user interface widgets require interactive elements and structural controls, which risk behavioral divergence and visual inconsistency if developed ad hoc across different application modules. A standardized component library is required to govern foundational primitive composition.

## Decision

1. MUST: All composite user interface block components MUST import and compose user interface primitives and utilities exclusively from @openstatus/ui.

## Policy Block

- MUST All composite user interface block components MUST import and compose user interface primitives and utilities exclusively from @openstatus/ui.

In scope:
- All composite interface block components authored within the interface package hierarchy.
- Interactive switcher, navigation, and display widgets requiring standard primitive controls.

Out of scope:
- Standalone external micro-frontends or scripts that do not integrate with the shared application workspace.
- Low-level core primitive component implementations authored within the foundational design library itself.

## Rationale

- Empirical evidence across switcher components reveals unanimous adoption of @openstatus/ui primitives including buttons, dropdown menus, and loading skeletons.
- Centralizing primitive UI elements within @openstatus/ui guarantees predictable interaction contracts, consistent accessibility semantics, and centralized theming across all consuming blocks.
- Consuming shared utility modules from @openstatus/ui avoids redundant helper logic for class merging and styling manipulation.

## Consequences

Positive:
- Ensures unified visual aesthetics, accessibility compliance, and design token adherence across all composite block components.
- Accelerates feature development by assembling pre-tested, standardized atomic primitives rather than reimplementing basic UI behaviors.
- Centralizes interface bug fixes and theme adjustments within a single shared module.

Negative:
- Direct coupling of composite widgets to @openstatus/ui necessitates synchronized updates when foundational primitive APIs change.
- Customizing non-standard component variants requires extending the upstream @openstatus/ui package rather than modifying local components in isolation.

## Alternatives

- Implement bespoke inline interactive elements within each composite block component (rejected)
  Rejected because: Duplicating core primitives leads to visual inconsistencies, fractures theme propagation, and increases maintenance overhead across the codebase.
  When valid: When prototyping temporary proof-of-concept components strictly isolated from production release packages.
- Import external third-party component libraries directly into individual composite block components (rejected)
  Rejected because: Bypassing @openstatus/ui fragments design tokens, complicates global styling updates, and introduces uncoordinated external dependencies across consumer packages.
  When valid: When authoring a foundational primitive package intended to be wrapped and exported through the unified design system.

## Risks

- Upstream API modifications within @openstatus/ui primitives can create breaking changes across consuming composite blocks.
  Mitigation: Maintain rigorous automated integration tests and adhere to semantic versioning contracts across internal workspace packages.
  Owner: Frontend engineering team
- Overly rigid primitive specifications could impede specialized composite block UX requirements.
  Mitigation: Provide flexible composition slots and configuration props on core primitives to satisfy varied block layouts without bypassing the library.
  Owner: Design systems team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that newly authored block components leverage existing primitives and utility functions from @openstatus/ui rather than defining separate styling helpers.
- Ensure that interactive components consuming @openstatus/ui primitives and managing client-side events reside within explicit client rendering boundaries.

## Continuation Context


Verify commands:
- Discover the project dependency manifest and verification scripts, then execute the workspace static analysis and linting suite to verify component import conformance.
- Discover and run the project type checker across all interface packages to validate component property and interface contract compliance.

Accept when:
- Static analysis validates that all composite interface primitives are imported directly from @openstatus/ui without unapproved direct third-party UI dependencies.
- The project type checking and component verification suites execute without errors across all consuming modules.

## Enforcement

- Verified by: Automated static analysis and linting checks configured in continuous integration pipelines.
- Verified by: Peer code review of component import paths during pull request evaluations.
- Violation handling: Pull requests introducing duplicate primitive implementations or unapproved external UI libraries will fail automated validation checks.
- Violation handling: Violations must be refactored to consume equivalent primitives from @openstatus/ui before merge.
- Exception process: Submit an architectural review request outlining the technical limitations of existing @openstatus/ui primitives.
- Exception process: Obtain sign-off from design systems maintainers before introducing alternative component patterns or direct dependencies.