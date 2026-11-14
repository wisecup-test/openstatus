---
version: 2
name: 3a7274a9-6ad6-4ccd-8951-b54c3a45d12f-adoption-of-openstatus-header-analysis-for-cdn-and-cache-header-evaluation-components-diagnostic-modules-analyzing-http-edge
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Components Diagnostic Modules Analyzing Http Edge"
metadata:
  actual:
    adr_id: 3a7274a9-6ad6-4ccd-8951-b54c3a45d12f
    status: accepted
    candidate_group_id: 4dc8ceff-793e-4bd6-86a1-3c4c1bee59c4
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/cdn-checker/**
---

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Components Diagnostic Modules Analyzing Http Edge

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/lib/cdn-checker/**`

## Context

- HTTP edge diagnostics and CDN analysis require parsing and interpreting heterogeneous provider-specific response headers.
- Inconsistent interpretation of cache status and provider signatures across network probes and frontend visualization creates discrepancies in diagnostic reporting.
- Multiple subsystems across probing, aggregation, and UI presentation require uniform access to header evaluation logic.

## Problem Statement

Dispersing HTTP response header evaluation and CDN signature matching across probe routines, summary aggregations, and visual presentation components leads to inconsistent classification of edge topology, cache states, and provider attributions.

## Decision

1. MUST: Components and diagnostic modules analyzing HTTP edge response headers, cache status, or CDN provider signatures MUST consume the @openstatus/header-analysis module as the single source of truth for header evaluation.

## Policy Block

- MUST Components and diagnostic modules analyzing HTTP edge response headers, cache status, or CDN provider signatures MUST consume the @openstatus/header-analysis module as the single source of truth for header evaluation.

In scope:
- All network probing services and edge diagnostic modules that evaluate HTTP response headers.
- All presentation components and data aggregation summaries that display CDN or cache evaluation results.

Out of scope:
- Generic network request utilities that do not evaluate CDN topology or cache performance indicators.
- Third-party monitoring adapters with self-contained proprietary reporting formats.

## Rationale

- Centralizing CDN signature detection and cache status classification within @openstatus/header-analysis ensures all diagnostic modules share identical classification logic.
- Prevents divergent parsing of provider-specific header conventions across network probe handlers and user-facing presentation components.
- Facilitates integration with standardized data validation schemas and reporting pipelines.

## Consequences

Positive:
- Consolidates edge POP, cache status, and CDN provider detection heuristics into a single maintainable boundary.
- Guarantees consistency in CDN topology and cache diagnosis across probe execution, summary aggregation, and user interface rendering.
- Enables declarative validation of network probe results against unified data contracts.

Negative:
- Upstream changes to CDN header conventions require coordinated updates within the shared header analysis module and its consumers.
- Introduces a cross-cutting module dependency across both presentation components and diagnostic probe layers.

## Alternatives

- Ad-hoc local header parsing within individual route handlers and UI components (rejected)
  Rejected because: Scatters provider header heuristics across UI and backend layers, resulting in divergent cache status interpretations and high maintenance overhead.
  When valid: Prototyping an isolated single-page test harness with no reliance on external provider detection standards.
- Delegation of header analysis entirely to external third-party network runtime APIs at runtime (rejected)
  Rejected because: Introduces external network latency and external runtime dependencies for deterministic header parsing tasks.
  When valid: When localized edge header heuristics cannot be maintained internally.

## Risks

- Provider-specific header updates could break detection heuristics across all dependent components.
  Mitigation: Maintain comprehensive fixture-based unit tests for all provider header variations within the shared analysis package.
  Owner: engineering team
- Including header analysis routines in client-side bundles could increase client bundle footprint.
  Mitigation: Ensure header analysis utilities avoid heavy runtime dependencies so that client component bundles remain lightweight.
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
- Consume shared header analysis routines rather than manipulating raw response header maps directly within presentation or probing modules.
- Couple header analysis outputs with schema validation schemas to enforce runtime data contracts.

## Continuation Context


Verify commands:
- Discover and execute the repository dependency audit and type checking verification scripts.
- Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.

Accept when:
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

## Enforcement

- Verified by: Automated dependency and import boundary verification in continuous integration workflows.
- Verified by: Peer architecture review on pull requests touching network diagnostic layers or presentation components.
- Violation handling: Build rejection on direct or duplicated header parsing implementations outside the canonical library.
- Violation handling: Mandatory refactoring of unapproved header heuristic implementations to consume the shared package.
- Exception process: Submit an architectural RFC detailing why the standardized header analysis package cannot fulfill domain requirements.
- Exception process: Obtain sign-off from domain architects prior to implementing any independent header inspection logic.