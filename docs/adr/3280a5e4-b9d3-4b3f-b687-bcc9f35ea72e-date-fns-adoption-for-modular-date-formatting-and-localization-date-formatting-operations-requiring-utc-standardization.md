---
version: 2
name: 3280a5e4-b9d3-4b3f-b687-bcc9f35ea72e-date-fns-adoption-for-modular-date-formatting-and-localization-date-formatting-operations-requiring-utc-standardization
description: "date-fns Adoption for Modular Date Formatting and Localization: Date Formatting Operations Requiring Utc Standardization"
metadata:
  actual:
    adr_id: 3280a5e4-b9d3-4b3f-b687-bcc9f35ea72e
    status: accepted
    candidate_group_id: 96fcb868-41ab-47cd-9c75-3a2bf49f4c7d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
      - src/lib/**
---

# date-fns Adoption for Modular Date Formatting and Localization: Date Formatting Operations Requiring Utc Standardization

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/**`
- `src/lib/**`

## Context

- Applications displaying operational metrics and time-series telemetry require consistent temporal parsing, formatting, and internationalization.
- Temporal display logic requires explicit UTC alignment to prevent timezone drift across distributed client and server environments.
- Direct reliance on unstandardized runtime date handling leads to inconsistent formatting and fragmented locale handling.
- Centralizing date manipulation around a functional date library standardizes timestamp transformations and locale-sensitive rendering.

## Problem Statement

Handling date transformations, timezone normalization, and internationalized display across formatting utilities and client presentation components introduces risk of inconsistent date rendering and fragmented locale handling when unstandardized.

## Decision

1. MUST: Date formatting operations requiring UTC standardization MUST normalize timestamps through dedicated UTC utility wrappers before applying presentation formatting.

## Policy Block

- MUST Date formatting operations requiring UTC standardization MUST normalize timestamps through dedicated UTC utility wrappers before applying presentation formatting.

In scope:
- Utility formatting modules handling timestamp and date transformations.
- Client-side user interface components rendering temporal data or localized timestamps.

Out of scope:
- Database persistence layers handling native database timestamp types.
- External protocol transport schemas exchanging raw ISO-8601 strings.

## Rationale

- Functional date manipulation via date-fns ensures pure, immutable date transformations without prototype pollution or mutable state bugs.
- Segregating UTC parsing and client locale configuration decouples temporal data normalization from regional presentation preferences.
- Standardizing on modular functional imports optimizes bundle footprint and eliminates divergent formatting implementations across presentation components.

## Consequences

Positive:
- Ensures uniform date formatting and timezone handling across application presentation layers.
- Improves modularity and bundle efficiency by importing individual functional date utilities.
- Centralizes localization configuration through dedicated client provider boundaries.

Negative:
- Requires client presentation components to reside within the date provider hierarchy to access localization contexts.
- Introduces dependency coordination between core date utility packages and locale companion modules.

## Alternatives

- Standardizing exclusively on native runtime Date and Intl APIs without third-party utilities (rejected)
  Rejected because: Native APIs require verbose boilerplate for complex date arithmetic, relative formatting, and UTC conversions, leading to duplicated bespoke formatting logic across components.
  When valid: Lightweight applications with minimal date formatting needs and no requirement for shared functional date utilities.
- Adopting monolithic object-oriented date libraries with mutable state (rejected)
  Rejected because: Object-oriented mutable date libraries carry significant bundle overhead, lack granular tree-shaking, and introduce mutable date state bugs across application boundaries.
  When valid: Legacy systems with existing entrenched object-oriented date handling models.

## Risks

- Inadvertent omission of UTC normalization leading to discrepancies between server-generated timestamps and client local time.
  Mitigation: Enforce UTC normalization helper usage in centralized formatting utilities prior to presentation.
  Owner: Frontend engineering team
- Bundle size regression if multiple locale bundles are imported eagerly instead of scoped through provider configurations.
  Mitigation: Restrict locale configurations to dynamically imported or centrally managed provider modules.
  Owner: Frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Structure formatting functions into reusable utility modules that accept timestamps and return formatted strings using imported date-fns helpers.
- Ensure client presentation trees are wrapped with the date provider component to supply locale configurations to child date displays.

## Continuation Context


Verify commands:
- Discover the project test execution script from the repository manifest and execute unit tests covering date formatting utilities.
- Discover the project static analysis and linting script from the repository manifest and run validation to verify absence of disallowed direct date mutations.
- Discover the project type verification script from the repository manifest and run type checks across client provider components.

Accept when:
- All date formatting utility test suites execute successfully with consistent UTC and localized outputs.
- Static analysis reports zero violations for unapproved date manipulation libraries or direct mutations.
- Type checking passes without errors across all formatting modules and provider components.

## Enforcement

- Verified by: Automated test suites validating date formatting behavior across simulated timezones.
- Verified by: Static analysis checks configured in continuous integration pipelines to detect unauthorized date libraries.
- Verified by: Peer code review for pull requests modifying date formatting or provider configuration.
- Violation handling: Pull requests introducing unapproved date libraries or bypassing standardized date utilities are blocked at code review.
- Violation handling: Continuous integration failures on date formatting tests or linting rules halt pipeline progression until resolved.
- Exception process: Submit an architectural review request detailing why date-fns cannot satisfy specific specialized timing or scheduling requirements.
- Exception process: Secure formal sign-off from frontend engineering leads before introducing isolated date manipulation alternatives.