---
version: 2
name: e3e05c7f-da35-4f23-9e51-9a0ce4769d11-adoption-of-openstatus-header-analysis-for-http-header-analysis-developers-integrating-updating-header-analysis-dependency
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Developers Integrating Updating Header Analysis Dependency"
metadata:
  actual:
    adr_id: e3e05c7f-da35-4f23-9e51-9a0ce4769d11
    status: accepted
    candidate_group_id: 523298a7-920b-4326-9c15-e24d08a53935
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/cdn-checker/**
---

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Developers Integrating Updating Header Analysis Dependency

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/lib/cdn-checker/**`

## Context

- Diagnostic probe routines fetch edge endpoint responses and extract HTTP headers to evaluate content delivery network performance and behavior.
- Presentation layers require structured header analysis metrics to display diagnostic summaries and status cards to users.
- Without a unified header evaluation module, probing logic and presentation components risk implementing divergent header parsing rules or duplicating header inspection logic.
- The project isolates header parsing and classification into the dedicated @openstatus/header-analysis module consumed across diagnostic probe workflows and client UI components.

## Problem Statement

Multiple components across backend network probing and client-side presentation require accurate inspection and categorization of HTTP headers returned by content delivery networks. Divergent, ad-hoc header parsing across these tiers leads to inconsistent diagnostic evaluations, duplicate parsing logic, and maintenance overhead when header specifications evolve.

## Decision

1. MUST: Developers integrating or updating the header analysis dependency MUST discover the repository resolution lock artifact and verify that the exact locked version satisfies compatibility requirements before modifying code.

## Policy Block

- MUST Developers integrating or updating the header analysis dependency MUST discover the repository resolution lock artifact and verify that the exact locked version satisfies compatibility requirements before modifying code.

In scope:
- Diagnostic network probe routines that evaluate HTTP response headers from target endpoints.
- User interface components that render CDN diagnostic metrics and header analysis summaries.
- Shared utility modules coordinating diagnostic checks across network boundaries.

Out of scope:
- Network operations that do not inspect, categorize, or report HTTP response headers.
- Generic user interface components that display unstructured textual data without domain analysis.

Exceptions:
- EXC-20-001: A specialized external probe environment requires lightweight transport telemetry without header categorization.

## Rationale

- Centralizing header parsing logic in @openstatus/header-analysis ensures that backend diagnostic probes and frontend visualization components apply identical classification rules to response metadata.
- Extracting domain-specific header analysis into a dedicated shared module prevents duplicate regex routines and string manipulation logic across disparate service and client boundaries.
- Structured outputs from a shared header analysis library decouple presentation components from provider-specific variations in HTTP header naming conventions.

## Consequences

Positive:
- Consistent classification and evaluation of CDN response headers across backend probes and frontend components.
- Reduced code duplication by centralizing header parsing algorithms and constants in a single module.
- Simpler maintenance when adding support for new CDN provider headers or updating header validation rules.

Negative:
- Frontend and backend tiers share a direct dependency on @openstatus/header-analysis, requiring synchronized dependency resolution across workspace consumers.
- Breaking changes in the header analysis schema require coordinated updates across both probe routines and UI summary cards.

## Alternatives

- Implement localized header parsing and regex extraction directly within individual probe and UI components (rejected)
  Rejected because: Leads to drift between how probe engines evaluate edge behavior and how client cards display header metrics, while dramatically increasing parsing code duplication.
  When valid: Single-purpose standalone utility scripts with no corresponding presentation layer or shared diagnostic requirements.
- Adopt a third-party generic HTTP header inspection library from an external registry (rejected)
  Rejected because: Generic third-party inspection utilities lack domain-specific CDN categorization, custom header schemas, and platform-aligned diagnostic conventions.
  When valid: Projects requiring only RFC-standard HTTP header validation without CDN-specific classification or custom diagnostic models.

## Risks

- Schema updates in @openstatus/header-analysis may break consumers in presentation components if contract versions drift.
  Mitigation: Enforce workspace-wide type verification and automated integration testing across both probe logic and client components prior to merging updates.
  Owner: Diagnostic engineering team
- Large or malformed HTTP response headers from probed endpoints could cause unexpected exceptions or performance degradation during analysis.
  Mitigation: Implement defensive input validation and payload bounds checking within the shared analysis module to guarantee safe handling of irregular header values.
  Owner: Diagnostic engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure probe implementations pass sanitized header dictionaries into the analysis module, leveraging shared checker utility helpers for common data normalization.
- Client components rendering diagnostic summaries should consume typed analysis models returned by the analysis module to prevent tight coupling to raw HTTP header strings.

## Continuation Context


Verify commands:
- Discover and execute the repository type verification script to ensure all consumers of the header analysis module conform to exported type signatures.
- Discover and run the project test suite covering network probe execution and diagnostic summary card rendering.
- Inspect module dependency declarations across the repository to verify that header inspection dependencies resolve consistently.

Accept when:
- All diagnostic network probes and UI summary components successfully import and consume @openstatus/header-analysis without type errors.
- Automated test suites covering probe execution and header classification pass cleanly.
- No ad-hoc header parsing routines or duplicate regex extractors exist outside the designated header analysis module.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type verification and test suites on pull requests.
- Verified by: Peer code reviews auditing network probe additions and UI components for direct imports of the shared analysis module.
- Violation handling: Pull requests containing ad-hoc header parsing or unapproved duplicate extraction routines are blocked until refactored to use the shared module.
- Violation handling: Static analysis and type checking failures trigger immediate continuous integration build rejection.
- Exception process: Submit an architectural review request detailing technical constraints preventing adoption of the shared analysis module.
- Exception process: Obtain explicit written approval from diagnostic platform maintainers and document the variance in module documentation.