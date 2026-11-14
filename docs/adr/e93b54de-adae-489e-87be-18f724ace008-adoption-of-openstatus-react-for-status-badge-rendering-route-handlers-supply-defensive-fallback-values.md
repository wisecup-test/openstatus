---
version: 2
name: e93b54de-adae-489e-87be-18f724ace008-adoption-of-openstatus-react-for-status-badge-rendering-route-handlers-supply-defensive-fallback-values
description: "Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Supply Defensive Fallback Values"
metadata:
  actual:
    adr_id: e93b54de-adae-489e-87be-18f724ace008
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Supply Defensive Fallback Values

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`

## Context

- Public badge route endpoints deliver dynamic status indicators configured through request query parameters.
- Generating consistent badge graphics requires shared UI components and visual representations across endpoint versions.
- Static analysis identifies @openstatus/react as the primary core library dependency across public badge routing endpoints.

## Problem Statement

Generating status badges across multiple endpoint revisions without a shared presentation module leads to duplicated markup, divergent styling, and maintenance fragmentation.

## Decision

1. MUST: Route handlers MUST supply defensive fallback values when requested parameter values are missing or invalid.

## Policy Block

- MUST Route handlers MUST supply defensive fallback values when requested parameter values are missing or invalid.

In scope:
- Public status page badge route handlers responsible for generating dynamic badge responses.
- Server-side endpoints rendering status indicator graphics from incoming query parameters.

Out of scope:
- Internal utility functions that do not render user-facing status indicators.
- Static document pages not involved in dynamic badge generation.

## Rationale

- Static analysis confirms the consistent import and application of @openstatus/react across public badge routes.
- Standardizing on a dedicated status component library enforces visual consistency and simplifies parameterization.
- Decoupling query parsing in route handlers from visual rendering logic preserves clean boundary separation.

## Consequences

Positive:
- Centralized visual definitions ensure consistent status representation across all badge endpoints.
- Parameters for visual customization such as theme and dimensions map directly to standardized component props.
- Eliminates duplicated markup and styling logic across disparate badge route handlers.

Negative:
- Route handlers become dependent on the @openstatus/react component interface lifecycle and API updates.
- Server-side rendering overhead increases compared to serving purely pre-generated static image assets.

## Alternatives

- In-handler inline SVG and HTML generation without dedicated component libraries (rejected)
  Rejected because: Duplicating visual rendering templates across independent badge endpoints creates maintenance overhead and styling inconsistencies.
  When valid: Valid for minimal microservices with single static response requirements and zero shared design system constraints.
- Generic external badge generation service redirect (rejected)
  Rejected because: Delegating badge generation introduces external network latency, third-party dependency risks, and loses direct theme coordination.
  When valid: Valid when hosting environments lack server-side rendering capabilities for dynamic graphics.

## Risks

- Breaking changes in upstream @openstatus/react component contracts may disrupt badge rendering.
  Mitigation: Pin dependencies via repository lock artifacts and verify upstream API compatibility before applying updates.
  Owner: engineering team
- Unvalidated or malformed search parameter values passed to components could cause server rendering errors.
  Mitigation: Implement strict parameter sanitization and define default fallbacks for all input parameters.
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
- Ensure request search parameter parsing extracts theme, size, and variant properties safely before passing them to badge components.
- Encapsulate fallback default values for missing or invalid query parameters to ensure predictable badge output.

## Continuation Context


Verify commands:
- Find and execute the repository validation script defined in the project configuration to confirm type checking and build integrity for badge route handlers.
- Run the test suite discovered from the package configuration to verify badge route handler responses and parameter resolution.

Accept when:
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

## Enforcement

- Verified by: Automated dependency and type verification during continuous integration checks.
- Verified by: Peer code review on pull requests touching status badge route handlers.
- Violation handling: Pull requests introducing inline badge markup or unapproved rendering libraries will be blocked from merging.
- Violation handling: Violations identified in code reviews require refactoring to use @openstatus/react prior to approval.
- Exception process: Submit an architectural exception proposal documenting the rationale for alternative rendering solutions.
- Exception process: Obtain explicit approval from the core engineering team before merging non-standard rendering handlers.