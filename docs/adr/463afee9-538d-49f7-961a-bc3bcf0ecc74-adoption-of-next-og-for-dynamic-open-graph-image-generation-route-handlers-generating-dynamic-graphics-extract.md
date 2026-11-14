---
version: 2
name: 463afee9-538d-49f7-961a-bc3bcf0ecc74-adoption-of-next-og-for-dynamic-open-graph-image-generation-route-handlers-generating-dynamic-graphics-extract
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Route Handlers Generating Dynamic Graphics Extract"
metadata:
  actual:
    adr_id: 463afee9-538d-49f7-961a-bc3bcf0ecc74
    status: accepted
    candidate_group_id: 6c49511a-9093-466c-988d-7593d1c959e4
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
      - src/lib/checker/**
---

# Adoption of next/og for Dynamic Open Graph Image Generation: Route Handlers Generating Dynamic Graphics Extract

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/og/**`
- `src/lib/checker/**`

## Context

- Dynamic Open Graph and social preview images require programmatic generation based on request query parameters such as entity identifiers, titles, and descriptions.
- Relying on disparate external image rendering services introduces third-party network latency, recurring service costs, and inconsistent visual branding across endpoints.
- The codebase establishes dynamic image rendering endpoints utilizing next/og alongside shared basic layout components to produce uniform metadata cards at the server runtime boundary.

## Problem Statement

Generating consistent, performant, and parameterized social card images across web application endpoints without standard tooling causes fragmentation, increased latency, and potential visual regressions across shared brand assets.

## Decision

1. MUST: Route handlers generating dynamic graphics MUST extract runtime parameters through request search parameter accessors and validate input parameters before passing them to the image template.

## Policy Block

- MUST Route handlers generating dynamic graphics MUST extract runtime parameters through request search parameter accessors and validate input parameters before passing them to the image template.

In scope:
- API endpoints and server handlers dedicated to rendering dynamic Open Graph and social preview image assets
- Modules providing visual layout components specifically designed for dynamic metadata graphics

Out of scope:
- Static image assets served directly from asset storage or static distribution networks
- Non-visual API endpoints delivering JSON, text, or stream payloads

Exceptions:
- EXC-20-001: A specialized graphic requires native canvas operations or third-party binary rendering unsupported by the edge image generation engine

## Rationale

- Adopting next/og enables colocation of image markup and business logic within existing route structures using familiar component paradigms.
- Generating images at the application runtime boundary eliminates reliance on external rendering APIs, lowering network latency and vendor coupling.
- Evidence in metadata image endpoints confirms a standard pattern of reading URL query parameters and rendering standardized basic layout components through next/og.

## Consequences

Positive:
- Centralizes and standardizes Open Graph card generation across all application routes.
- Eliminates external image rendering service dependencies and unnecessary network hops.
- Enables reusability of shared layout components across multiple distinct image endpoint routes.

Negative:
- Layout complexity and styling capabilities are constrained by the edge image layout engine subset.
- On-demand image synthesis introduces potential compute overhead and latency spikes on initial cache misses.

## Alternatives

- Headless browser screenshot service (rejected)
  Rejected because: Requires substantial server resource overhead, high cold-start latency, and complex infrastructure maintenance.
  When valid: When rendering complex full-page DOM layouts requiring full CSS and JavaScript execution engines.
- Pre-generated static image cards (rejected)
  Rejected because: Cannot accommodate dynamic query parameters, user-generated content, or real-time status data in social cards.
  When valid: When content and metadata are completely static and known ahead of time at build time.

## Risks

- Unsupported styling or font formatting within the image template can cause runtime rendering failures.
  Mitigation: Constrain template styling to verified edge layout properties and validate fallback fonts in shared layout components.
  Owner: frontend engineering team
- Malformed or oversized search query parameters could trigger denial-of-service or memory spikes during image generation.
  Mitigation: Validate and sanitize all search parameters before parsing and rendering template contents.
  Owner: platform engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Structure image templates using component hierarchies that share common visual styling tokens across endpoint routes.
- Ensure query parameters consumed for dynamic image rendering provide fallback defaults to prevent rendering failure on missing values.

## Continuation Context


Verify commands:
- Discover the workspace test execution script in the project manifest and run endpoint integration tests targeting image generation routes.
- Identify the workspace type-checking and linting configuration from the build setup and run validation across image endpoint modules.

Accept when:
- All dynamic image route endpoints resolve next/og imports and compile without type or layout errors.
- Automated test suites verify image endpoints return appropriate content-type headers and handle missing query parameters gracefully.

## Enforcement

- Verified by: Continuous integration type checking and automated route integration tests.
- Verified by: Peer code review verification of new image rendering endpoints ensuring compliance with shared layout and module standards.
- Violation handling: Pull requests introducing unapproved rendering mechanisms or unvalidated parameter handling will be blocked during automated review.
- Exception process: Submit an architectural review request outlining technical constraints that preclude using the standard image generation module.