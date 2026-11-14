---
version: 2
name: cdaf9bc2-582c-4435-99d2-17aa652a298d-internal-module-adoption-openstatus-ui-components-blocks-status-types-domain-contract-components-consuming-internationalization-27d9e2f7d8fcca8a
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Components Consuming Internationalization Utilities Status Formatting"
metadata:
  actual:
    adr_id: cdaf9bc2-582c-4435-99d2-17aa652a298d
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Components Consuming Internationalization Utilities Status Formatting

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- The UI package provides status monitoring block components including status bars, event timelines, calendars, banners, feeds, and indicators.
- Visual components and accompanying data calculation utilities require a consistent domain schema representing status values, event markers, and operational metrics.
- Decentralized type declarations across individual presentation components lead to interface drift, inconsistent data transformation, and brittle refactoring.
- Centralizing shared type definitions in the dedicated internal module @openstatus/ui/components/blocks/status.types establishes a single source of truth across all status block modules.

## Problem Statement

Status monitoring user interfaces comprise multiple interrelated components that visualize identical domain concepts across different temporal and structural layouts. Without an isolated domain contract module, component authors risk declaring divergent prop interfaces and data structures, resulting in runtime conversion errors, tight coupling between visual components, and circular dependency cycles across helper utilities.

## Decision

1. SHOULD: Components consuming internationalization utilities for status formatting SHOULD coordinate localized translation keys with the domain variants declared in @openstatus/ui/components/blocks/status.types.

## Policy Block

- SHOULD Components consuming internationalization utilities for status formatting SHOULD coordinate localized translation keys with the domain variants declared in @openstatus/ui/components/blocks/status.types.

In scope:
- Status block visual components within the UI package
- Utility and internationalization helper modules operating on status data

Out of scope:
- Generic UI primitives that do not represent status domain data
- External consumer applications that ingest compiled component packages without modifying block contracts

## Rationale

- Static analysis identifies @openstatus/ui/components/blocks/status.types imported across all eleven status block modules, demonstrating an established architectural boundary for status domain types.
- Isolating domain contracts into a headless type module enables pure utility functions and client rendering components to share identical schemas without introducing circular dependencies.
- Centralized type definitions enforce semantic consistency across disparate status visualizers, ensuring unified status enum variants and data structures.

## Consequences

Positive:
- Eliminates type divergence between status presentation blocks and domain data calculations.
- Decouples domain data modeling from presentation rendering logic and third-party UI component dependencies.
- Simplifies maintenance by providing a single canonical location for updating status domain schema changes.

Negative:
- Changes to shared type signatures in the central module can trigger downstream compilation errors across all consumer components.
- Increases import coupling for simple components that require only a subset of domain definitions.

## Alternatives

- Component-Local Type Definitions (rejected)
  Rejected because: Declaring types locally within each presentation component leads to duplicate schemas, interface drift, and type mismatch bugs when passing domain objects between blocks.
  When valid: Valid only in self-contained single-component prototypes with no shared domain dependencies.
- Colocating Domain Types Inside a Primary Component Module (rejected)
  Rejected because: Exporting domain types from a primary visual component creates circular dependencies when utility modules or secondary components require the types.
  When valid: Valid when a component has no external helper utilities or sibling component consumers.

## Risks

- Breaking changes in the shared domain types module immediately propagate across all dependent status block components.
  Mitigation: Employ comprehensive type verification suites and maintain backwards-compatible optional properties during interface evolution.
  Owner: engineering team
- Accidental introduction of runtime visual dependencies into the type contract module creating circular references.
  Mitigation: Enforce type-only exports and ban runtime presentation imports within the types module through automated module boundary checks.
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
- Ensure that modules consuming @openstatus/ui/components/blocks/status.types use type-only imports where possible to optimize compilation and prevent runtime bundle bloat.
- Coordinate date and timestamp representations defined in the domain contracts with centralized date utility functions to guarantee uniform temporal formatting.

## Continuation Context


Verify commands:
- find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec grep -l "@openstatus/ui/components/blocks/status.types" {} +
- echo "Discover repository build configuration and execute the static type verification suite to ensure contract compliance across status blocks"

Accept when:
- Static type analysis passes with zero type errors across all status block components and utility modules.
- All status block components source shared domain models exclusively from @openstatus/ui/components/blocks/status.types with no duplicate local domain interfaces.

## Enforcement

- Verified by: Continuous integration static type verification suites
- Verified by: Peer code review auditing component prop definitions against canonical domain types
- Violation handling: Pull requests containing duplicate local status domain models or bypasses of @openstatus/ui/components/blocks/status.types are blocked from merging.
- Exception process: Exceptions require approval from the UI architecture team and documented rationale justifying isolation from the shared status domain contract.