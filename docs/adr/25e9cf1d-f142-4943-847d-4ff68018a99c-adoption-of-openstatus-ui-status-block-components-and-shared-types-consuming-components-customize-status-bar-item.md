---
version: 2
name: 25e9cf1d-f142-4943-847d-4ff68018a99c-adoption-of-openstatus-ui-status-block-components-and-shared-types-consuming-components-customize-status-bar-item
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Consuming Components Customize Status Bar Item"
metadata:
  actual:
    adr_id: 25e9cf1d-f142-4943-847d-4ff68018a99c
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Consuming Components Customize Status Bar Item

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`

## Context

- Frontend status dashboards require consistent visual representations of service health, incident events, and historical uptime metrics across multiple views.
- Uncoordinated status visualizations lead to divergent severity indicators, inconsistent time-interval bucketing, and duplicated type declarations.
- The workspace contains a dedicated UI library module providing reusable status blocks and standardized type definitions.

## Problem Statement

Without a centralized component and type contract, different status views implement ad-hoc incident models and bespoke timeline visualizations. This creates visual inconsistency across service dashboards, fragments incident severity interpretations, and duplicates time-series bucketing logic across frontend modules.

## Decision

1. MAY: Consuming components MAY customize status bar item appearance using supported component slot selectors and utility classes when specific layout variations are required.

## Policy Block

- MAY Consuming components MAY customize status bar item appearance using supported component slot selectors and utility classes when specific layout variations are required.

In scope:
- Status dashboard pages, incident timeline views, and service history displays.
- Data aggregation functions that map telemetry and incident records into daily status intervals.

Out of scope:
- Backend data ingestion pipelines that do not render user interface components.
- Generic UI primitives that do not represent service status, health indicators, or incident metrics.

Exceptions:
- EXC-001: Rendering legacy service views that require backward-compatible markup during active migration periods

## Rationale

- Centralizing status bar components and incident data structures within @openstatus/ui enforces uniform styling and behavioral consistency across all status surfaces.
- Shared type contracts guarantee that incident data transformations, severity calculations, and rendering blocks remain strictly aligned across data preparation and presentation layers.
- Codebase evidence across multiple status pages demonstrates established adoption of @openstatus/ui blocks for incident visualization.

## Consequences

Positive:
- Standardizes status timeline rendering and incident domain types across all web presentation interfaces.
- Eliminates duplication of aggregation, severity calculation, and status visualization logic.
- Enforces consistent data modeling between incident aggregation routines and visual status bar elements.

Negative:
- Consuming applications are tightly coupled to the component interface contracts published by @openstatus/ui.
- Breaking changes to shared status type definitions require coordinated updates across all downstream consumers.
- Custom styling overrides require targeted slot targeting rather than localized independent markup.

## Alternatives

- Locally defining status types and bar components within each consuming application module (rejected)
  Rejected because: Creates fragmented type definitions, divergent styling, and duplicate maintenance of status calculation logic across presentation pages
  When valid: When building an isolated prototype that does not integrate with shared workspace status contracts
- Adopting an unstyled third-party charting library for status bars (rejected)
  Rejected because: Introduces external dependency overhead without domain-specific incident aggregation contracts or design token alignment
  When valid: When rendering arbitrary multi-dimensional time series data that falls outside the status domain model

## Risks

- A breaking API change in @openstatus/ui status blocks can disrupt multiple consumer pages simultaneously.
  Mitigation: Maintain strict semantic versioning and comprehensive component regression tests within the UI package before publishing updates.
  Owner: Platform Engineering Team
- Unique status dashboard layout requirements may lead developers to bypass shared components if customization is overly constrained.
  Mitigation: Expose extensible slot properties and modular helper utilities to allow custom layout composition without forking base components.
  Owner: Frontend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import status types and status bar components using canonical module resolution paths provided by the UI package.
- Ensure client components isolate stateful interactivity while maintaining purity in underlying status aggregation functions.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script across all workspace packages to ensure compliance with shared type contracts.
- Discover and run the repository linter to detect unapproved local status type definitions or bypassed package imports.
- Execute the automated test suite to verify correct rendering and aggregation of status timeline components.

Accept when:
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

## Enforcement

- Verified by: Automated type checking across all workspace packages during pull request verification.
- Verified by: Linting rules restricting ad-hoc status type definitions outside the designated shared module.
- Verified by: Peer code review confirming adherence to shared component contracts.
- Violation handling: Pull requests introducing duplicate status domain types or localized status bar implementations fail automated validation.
- Violation handling: Reviewers require migration to the canonical @openstatus/ui components and type definitions before merging.
- Exception process: Exceptions require architectural review by the frontend platform maintainers.
- Exception process: Approved exceptions must document the technical constraints preventing adoption of shared status blocks alongside a migration plan.