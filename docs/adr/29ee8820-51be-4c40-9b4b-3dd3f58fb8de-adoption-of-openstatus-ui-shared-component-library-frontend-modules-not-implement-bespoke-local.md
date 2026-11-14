---
version: 2
name: 29ee8820-51be-4c40-9b4b-3dd3f58fb8de-adoption-of-openstatus-ui-shared-component-library-frontend-modules-not-implement-bespoke-local
description: "Adoption of @openstatus/ui Shared Component Library: Frontend Modules Not Implement Bespoke Local"
metadata:
  actual:
    adr_id: 29ee8820-51be-4c40-9b4b-3dd3f58fb8de
    status: accepted
    candidate_group_id: 31903934-689a-42b8-bdae-1bedc27d1b4a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Frontend Modules Not Implement Bespoke Local

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/components/**`

## Context

- Frontend applications require consistent visual components, status indicators, and layouts across diverse monitoring and presentation views.
- Analysis of twenty-nine application modules demonstrates widespread imports from the internal @openstatus/ui package for UI primitives, composite blocks, and helper routines.
- Centralizing common interface elements into a shared workspace library eliminates styling fragmentation and redundant component maintenance.

## Problem Statement

Decentralized presentation logic and duplicate visual primitives across frontend routes introduce styling inconsistencies, redundant maintenance overhead, and divergent user experience. The application requires a centralized, standardized component library that provides authoritative interface primitives, layout blocks, and utility helpers across all client-facing modules.

## Decision

1. MUST_NOT: Frontend modules MUST NOT implement bespoke local duplicates of primitives or utility helpers already provided by @openstatus/ui.

## Policy Block

- MUST_NOT Frontend modules MUST NOT implement bespoke local duplicates of primitives or utility helpers already provided by @openstatus/ui.

In scope:
- User interface components, page layouts, and interactive presentation views within frontend applications.
- Shared presentation utility functions and UI helper routines across client-facing modules.

Out of scope:
- Backend services, database access schemas, and server-side data extraction routines.
- Command-line tools, background worker scripts, and non-visual infrastructure packages.

Exceptions:
- EX-20-001: A specialized charting or canvas surface requires low-level visual rendering primitives not exposed by the shared UI library

## Rationale

- Centralizing UI primitives and blocks in @openstatus/ui guarantees visual and behavioral consistency across status page views and public interfaces.
- Extracting shared utilities to @openstatus/ui/lib/utils eliminates duplicated styling and class-merging helper code across frontend applications.
- Evidence across 29 files demonstrates widespread adoption of @openstatus/ui for buttons, tabs, skeletons, charts, metric cards, and layout containers.

## Consequences

Positive:
- Guarantees visual consistency, shared theme compliance, and predictable UI behavior across all consumer views.
- Reduces code duplication across applications by maintaining single sources of truth for interface components and utility functions.
- Accelerates frontend feature development through standardized composite blocks and reusable primitives.

Negative:
- Creates an architectural coupling between application presentation layers and the internal UI package schema.
- Modifications to shared primitives in @openstatus/ui can introduce breaking changes across multiple consuming application routes.

## Alternatives

- Author bespoke UI primitives and styling utilities locally within each consuming application directory (rejected)
  Rejected because: Leads to fragmented user experience, styling divergence, and maintenance overhead across application boundaries
  When valid: Valid only in standalone throwaway prototypes or isolated experimental modules with no shared design requirements
- Import third-party visual component libraries and styling utilities directly into application presentation components (rejected)
  Rejected because: Prevents centralized design token enforcement, consistent accessible encapsulation, and shared theme customization
  When valid: Valid when an application requires an isolated visual paradigm entirely detached from core product identity

## Risks

- Breaking changes in @openstatus/ui components or utility interfaces can cascade across multiple consumer modules
  Mitigation: Enforce strict semantic versioning and run workspace automated test validation suites before publishing updates
  Owner: Frontend engineering team
- Overly rigid composite blocks in @openstatus/ui may impede domain-specific UI customization
  Mitigation: Design primitives and blocks with flexible composition slots and expose configurable variant properties
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
- When introducing new user interface elements, verify whether existing primitives or composite blocks within @openstatus/ui meet presentation requirements prior to authoring custom structures.
- Ensure all styling merges and dynamic class assignments utilize helper routines imported directly from @openstatus/ui/lib/utils.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and type verification script in the repository manifest and execute it across all frontend packages.
- Discover the test runner execution script defined in the project configuration and execute the component test suite.
- Discover the repository import boundary and linting check command to verify compliance with module import paths.

Accept when:
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

## Enforcement

- Verified by: Continuous integration automated static analysis, linting passes, and type checking
- Verified by: Peer code review verification of presentation layer pull requests
- Violation handling: Pull requests introducing local duplicate primitives or improper import paths are blocked from merging until corrected
- Exception process: Submit an architectural review request outlining technical constraints that justify bypassing shared UI primitives, subject to design system lead approval