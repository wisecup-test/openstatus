---
version: 2
name: a7326c09-06bb-4f16-b6df-e76a6b293567-adoption-of-openstatus-ui-shared-component-library-frontend-modules-requiring-interactive-user-interface
description: "Adoption of @openstatus/ui Shared Component Library: Frontend Modules Requiring Interactive User Interface"
metadata:
  actual:
    adr_id: a7326c09-06bb-4f16-b6df-e76a6b293567
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Frontend Modules Requiring Interactive User Interface

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- Interactive dashboard surfaces across forms, operational views, and data tables require standardized input controls and selection mechanisms to maintain cohesive user experience.
- Decentralized or duplicate UI primitive implementations across multiple form modules risk visual drift, fragmented accessibility behaviors, and redundant styling definitions.
- The codebase adopts the internal @openstatus/ui workspace package to centralize atomic user interface primitives and provide uniform interaction semantics across frontend features.

## Problem Statement

Frontend feature modules and operational forms across the dashboard require uniform input controls, selection primitives, and layout elements. Without a shared component package, distinct form implementations risk divergent styling, inconsistent accessibility semantics, and duplicated maintenance effort across user-facing views.

## Decision

1. MUST: Frontend modules requiring interactive user interface controls MUST import shared design components directly from the internal @openstatus/ui module.

## Policy Block

- MUST Frontend modules requiring interactive user interface controls MUST import shared design components directly from the internal @openstatus/ui module.

In scope:
- User interface components, dashboard form screens, composite selection trees, and data table column views requiring standardized UI controls.

Out of scope:
- Backend data services, schema definitions without UI rendering, non-interactive static layout markup, and standalone API route handlers.

## Rationale

- Centralizing primitive controls in @openstatus/ui ensures visual and behavioral consistency across all application forms and operational tables.
- Modular component imports isolate consumer dependencies and facilitate bundle optimization across dashboard views.
- Reusing established component primitives reduces defect rates in form state binding and interactive selection handling.

## Consequences

Positive:
- Consistent user experience and standardized visual presentation across all dashboard forms and tabular views.
- Reduced boilerplate and duplication when constructing interactive selection controls and input fields.
- Centralized governance over component styling, accessibility standards, and state binding behaviors.

Negative:
- Coupling of feature modules to the internal component library API lifecycle and maintenance cadence.
- Need to coordinate updates across workspace boundaries when extending primitive component capabilities.

## Alternatives

- Implement native HTML form controls with ad-hoc styling directly within feature modules (rejected)
  Rejected because: Leads to fragmented visual styles, duplicated accessibility handling, and inconsistent user interactions across distinct forms
  When valid: Valid only in lightweight standalone prototyping environments lacking shared UI component infrastructure
- Adopt an external third-party component library directly in individual feature applications (rejected)
  Rejected because: Bypasses organization-specific styling conventions and limits customization across shared workspace modules
  When valid: Valid for isolated greenfield projects that do not share design tokens with the primary platform

## Risks

- Breaking changes in @openstatus/ui component interfaces may cause compile-time or runtime failures across consuming form views
  Mitigation: Require strict lock-file inspection, interface contract tests, and coordinated workspace verification prior to publishing component updates
  Owner: engineering team
- Over-reliance on monolithic imports could inflate bundle sizes if modular import boundaries are bypassed
  Mitigation: Enforce modular path imports and static analysis linting rules across consumer feature modules
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
- Consumers should locate the shared UI workspace package within the repository structure and import required interactive primitives using modular path specifiers.
- When composing multi-item selection controls or tree hierarchies, encapsulate grouping logic in dedicated composite components while delegating state rendering to library primitives.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script to verify module imports and component props conform to @openstatus/ui definitions.
- Discover and execute the project linting and structural analysis suite to ensure component imports adhere to modular path conventions.
- Discover and execute the automated test runner to validate form interaction and selection state handling across consuming views.

Accept when:
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

## Enforcement

- Verified by: Automated continuous integration pipelines validating type correctness and import path compliance.
- Verified by: Peer code review confirming the use of shared library primitives instead of custom raw input controls.
- Violation handling: Pull requests introducing duplicated primitive components or bypassing library imports are blocked until refactored.
- Violation handling: Type and lint failures generated by incompatible component props result in automated build failure.
- Exception process: Exceptions for specialized or bespoke controls must be submitted via architectural review with documented rationale and accessibility verification.