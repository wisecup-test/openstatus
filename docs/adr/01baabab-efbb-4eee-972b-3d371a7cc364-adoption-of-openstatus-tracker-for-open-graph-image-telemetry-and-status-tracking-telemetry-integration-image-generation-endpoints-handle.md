---
version: 2
name: 01baabab-efbb-4eee-972b-3d371a7cc364-adoption-of-openstatus-tracker-for-open-graph-image-telemetry-and-status-tracking-telemetry-integration-image-generation-endpoints-handle
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Telemetry Integration Image Generation Endpoints Handle"
metadata:
  actual:
    adr_id: 01baabab-efbb-4eee-972b-3d371a7cc364
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Telemetry Integration Image Generation Endpoints Handle

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/og/**`

## Context

- The Open Graph image generation subsystem generates dynamic status visuals in response to incoming route requests.
- Telemetry and status tracking require consistent instrumentation across the image route handler and constituent visual status components.
- Multiple components across the dynamic image generation route boundary share an import of the @openstatus/tracker library.

## Problem Statement

Rendering dynamic status graphics requires instrumenting telemetry without fragmenting tracking implementations across handlers and subcomponents. Without a standard tracker module, image generation endpoints risk inconsistent telemetry emission or duplicate tracking logic across visual status components.

## Decision

1. MUST: Telemetry integration in image generation endpoints MUST handle tracking failures gracefully without interrupting image response rendering.

## Policy Block

- MUST Telemetry integration in image generation endpoints MUST handle tracking failures gracefully without interrupting image response rendering.

In scope:
- Dynamic Open Graph image generation route handlers and associated visual status indicator components.

Out of scope:
- Static asset generation endpoints and application components operating outside the dynamic status image rendering pipeline.

## Rationale

- Adopting @openstatus/tracker centralizes event telemetry across route handlers and rendering components within the metadata generation pipeline.
- Standardizing on a single tracking package guarantees uniform metric attribution across status checks and Open Graph image requests.
- Reusing the shared tracking module across both image endpoints and subcomponents minimizes maintenance overhead and prevents divergent tracking mechanisms.

## Consequences

Positive:
- Ensures unified metric and event emission across status page image generation routes and subcomponents.
- Centralizes telemetry logic into a single dedicated module, reducing duplicated monitoring code.
- Decouples visual status component rendering from backend telemetry transport details.

Negative:
- Introduces an external dependency coupling inside dynamic metadata image generation endpoints.
- Requires coordinated interface updates across route handlers and visual components when updating the tracking library.

## Alternatives

- Ad-hoc telemetry logging via direct HTTP fetch calls in image generation components (rejected)
  Rejected because: Lacks structured schema validation and produces duplicate network telemetry logic across rendering components.
  When valid: Valid only in early standalone prototypes prior to adopting centralized telemetry libraries.
- Edge middleware request tracking without component-level telemetry instrumentation (rejected)
  Rejected because: Fails to capture granular status check context embedded within dynamic visual rendering components.
  When valid: Valid when only ingress HTTP path metrics are required without visual component context.

## Risks

- Tracker dispatch failures or latency spikes could delay or disrupt dynamic image response generation.
  Mitigation: Isolate telemetry dispatch with non-blocking error handling to ensure image rendering completes regardless of tracker availability.
  Owner: Engineering Team
- Breaking changes in the tracker library interface across dependency updates could break route compilation.
  Mitigation: Enforce strict lock artifact verification and automated interface checking in continuous integration pipelines.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that telemetry dispatches within rendering pipelines operate without blocking critical image response generation.
- Encapsulate tracker initialization and configuration within shared helper utilities to avoid configuration divergence across route handlers and components.

## Continuation Context


Verify commands:
- Discover the project dependency manifest and execute the designated typecheck script to verify module resolution for @openstatus/tracker.
- Execute the repository linting and test scripts discovered from the project configuration to confirm compliance across visual status components.

Accept when:
- All imports of @openstatus/tracker resolve cleanly during repository typecheck and build validation.
- Dynamic image generation route handlers and status components compile and pass all automated test suites without telemetry interface errors.

## Enforcement

- Verified by: Continuous integration typecheck and automated test pipeline execution.
- Verified by: Peer code review for all pull requests modifying image generation routes or status presentation components.
- Violation handling: Build failure on unresolved library references or unauthorized secondary tracker imports.
- Violation handling: Code review rejection requiring refactoring to use the standard @openstatus/tracker module interface.
- Exception process: Submit an architectural review request detailing technical constraints preventing standard tracker integration and documenting compensatory telemetry mechanisms.