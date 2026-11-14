---
version: 2
name: 125660f2-401d-4241-a82c-015ef52ae554-lucide-react-icon-library-adoption-icons-rendered-lucide-react-inherit-stroke
description: "lucide-react Icon Library Adoption: Icons Rendered Lucide React Inherit Stroke"
metadata:
  actual:
    adr_id: 125660f2-401d-4241-a82c-015ef52ae554
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Icons Rendered Lucide React Inherit Stroke

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/**`

## Context

- The design system provides reusable UI primitives including dialogs, menus, navigation structures, calendars, command palettes, and form inputs.
- Interactive primitives require visual affordances for directional cues, selection states, expansion chevrons, and operational status signs.
- Standardizing icon assets avoids visual divergence, duplicated vector geometry, and inconsistent sizing across component boundaries.
- Static analysis detects consistent iconography imports across core UI components and composite status blocks.

## Problem Statement

Rendering visual symbols using heterogeneous vector sources or ad-hoc inline markup introduces inconsistent stroke geometry, unpredictable scaling, accessibility gaps, and inflated bundle sizes across design system components. A standardized icon library is required to guarantee visual coherence, consistent developer ergonomics, and efficient asset tree-shaking across all UI primitives.

## Decision

1. SHOULD: Icons rendered from lucide-react SHOULD inherit stroke, sizing, and color attributes via utility classes or standard component props to maintain consistent design tokens across components.

## Policy Block

- SHOULD Icons rendered from lucide-react SHOULD inherit stroke, sizing, and color attributes via utility classes or standard component props to maintain consistent design tokens across components.

In scope:
- All UI component primitives and composite status blocks within the design system package requiring vector iconography.

Out of scope:
- Illustrations, multi-color marketing graphics, and complex brand marks that exceed single-color stroke icon specifications.

## Rationale

- Static analysis across the UI component package detects uniform adoption of lucide-react across twelve distinct component files.
- lucide-react provides React component wrappers with standardized stroke-based geometry, enabling uniform sizing and color inheritance via CSS utility classes.
- Named exports allow modern bundlers to tree-shake unreferenced icons, minimizing client bundle footprint compared to monolithic icon distributions.

## Consequences

Positive:
- Ensures uniform visual aesthetics, stroke geometry, and scaling behavior across all design system components.
- Enables fine-grained tree-shaking of individual named icon exports during bundle generation.
- Simplifies component authoring through standard React props for sizing, color, and accessibility attributes.

Negative:
- Introduces an external dependency coupling UI primitives to upstream icon taxonomy and naming conventions.
- Requires a deliberate fallback strategy when specialized domain symbols are absent from the upstream icon set.

## Alternatives

- Ad-hoc Inline SVG Definitions (rejected)
  Rejected because: Duplicating raw SVG XML paths directly in component markup inflates source file sizes, duplicates path data, and lacks unified prop interfaces.
  When valid: Valid only for isolated, bespoke brand marks or animations not represented in standard icon sets.
- SVG Sprite Sheet Architecture (rejected)
  Rejected because: Requires auxiliary asset pipeline orchestration and external symbol sheet loading, complicating component distribution across packages.
  When valid: Valid in server-rendered environments with stringent client script budgets that prohibit JavaScript-based icon rendering.

## Risks

- Upstream breaking changes or renamed icon identifiers across library releases.
  Mitigation: Enforce strict lock resolution and validate icon component identifiers during automated verification pipelines.
  Owner: UI Engineering Team
- Absence of specialized domain or status symbols in the upstream icon catalog.
  Mitigation: Establish a documented extension process for custom vector assets conforming to identical viewBox and stroke specifications.
  Owner: Design System Working Group

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify no unauthorized icon dependencies or inline SVG elements are introduced in UI components.
- Discover and execute the project build and type-checking scripts to confirm that all imported icon identifiers resolve successfully against the locked dependency version.

Accept when:
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

## Enforcement

- Verified by: Automated dependency analysis and static lint rules in continuous integration pipelines.
- Verified by: Peer review verification for all changes adding or updating UI component primitives.
- Violation handling: Pull requests introducing unapproved icon libraries or raw inline vector markup into UI components are rejected pending refactoring to lucide-react.
- Exception process: Exceptions for proprietary brand assets or specialized domain graphics require design system team review and explicit architectural approval in the pull request.