---
version: 2
name: b6d1bf6e-4e92-48d3-b0b3-4d1e8a60eada-adoption-of-date-fns-utc-for-utc-date-and-timestamp-management-modules-performing-timestamp-normalization-date-formatting
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Modules Performing Timestamp Normalization Date Formatting"
metadata:
  actual:
    adr_id: b6d1bf6e-4e92-48d3-b0b3-4d1e8a60eada
    status: accepted
    candidate_group_id: b78de858-5cfd-4774-802a-83e1d237e3bf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
      - src/lib/**
---

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Modules Performing Timestamp Normalization Date Formatting

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/content/**`
- `src/lib/**`

## Context

- Status monitoring and reporting interfaces require consistent, accurate temporal representation regardless of the viewing client local timezone.
- Formatting routines and interactive timestamp interface components handle date calculations that require strict UTC alignment.
- The project incorporates @date-fns/utc across shared formatting helpers and presentation components to enforce uniform timestamp processing.

## Problem Statement

In status monitoring systems, displaying timestamps according to local client timezones without deterministic normalization creates data inconsistencies across distributed users and leads to discrepancies between status metrics and reported incident timelines.

## Decision

1. MUST: Modules performing timestamp normalization, date formatting, and interactive temporal rendering MUST adopt @date-fns/utc for all UTC date handling operations.

## Policy Block

- MUST Modules performing timestamp normalization, date formatting, and interactive temporal rendering MUST adopt @date-fns/utc for all UTC date handling operations.

In scope:
- Utility modules responsible for parsing, converting, and formatting temporal data.
- User interface components that render status timestamps and interactive temporal hover cards.

Out of scope:
- Components and routines that explicitly format values for localized user-preference displays where local offsets are mandatory.
- Non-temporal status indicators and layout primitives that do not process date objects.

## Rationale

- Adopting @date-fns/utc centralizes UTC date representation, ensuring that status calculations and event timestamps remain identical across server and client rendering contexts.
- Direct evidence confirms repeated utilization across both data utility layers and user interface presentation components, showing deliberate standardization for date handling.
- Standardizing on a focused UTC extension avoids the maintenance overhead and error surface of custom timezone arithmetic.

## Consequences

Positive:
- Ensures uniform and deterministic timestamp calculations across utility formatters and user interface components.
- Eliminates timezone skew and hydration inconsistencies caused by differing local browser or client environments.
- Encapsulates temporal conversion logic to simplify status reporting and time-series data presentation.

Negative:
- Introduces an additional runtime and bundle dependency that must be tracked and kept compatible with core date utility libraries.
- Requires developers to adhere to standardized UTC normalization conventions rather than using platform-native date instances directly.

## Alternatives

- Rely on platform-native date objects without dedicated UTC normalization utilities (rejected)
  Rejected because: Platform-native date handling defaults to the host runtime timezone, causing display discrepancies between client environments and server timestamps.
  When valid: Valid in isolated environments where all operations are strictly localized or running on runtimes configured with fixed timezone offsets.
- Manual timezone offset arithmetic within each presentation component (rejected)
  Rejected because: Distributes timezone math across UI components, increasing code duplication and introducing edge-case calculation errors.
  When valid: Valid when timestamp manipulation is limited to a single non-reusable UI element with no global consistency requirement.

## Risks

- Incompatibility between @date-fns/utc and core date utility peer dependencies during library upgrades.
  Mitigation: Lock dependency versions strictly and verify API compatibility through project lock resolution artifacts prior to updating core date utility packages.
  Owner: engineering team
- Accidental use of native date constructors alongside UTC utilities leading to inconsistent hybrid timezone arithmetic.
  Mitigation: Establish shared formatting helpers that encapsulate UTC conversions, preventing unvetted direct date instantiations.
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
- Examine the project dependency configuration to identify shared date handling modules and establish reusable UTC helper functions.
- Ensure interactive components displaying status timestamps consume normalized values derived from @date-fns/utc rather than localized client date instances.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking and linting suites to ensure date manipulation modules comply with @date-fns/utc typing and import constraints.
- Discover and execute the repository automated test suites covering date formatting helpers and temporal component rendering.

Accept when:
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

## Enforcement

- Verified by: Automated static analysis checks verifying imports and usage across formatting and presentation modules.
- Verified by: Pull request code reviews ensuring new timestamp operations adopt the standardized library.
- Violation handling: Flag pull requests that introduce platform-dependent date arithmetic or bypass established UTC utility helpers.
- Violation handling: Require conversion to @date-fns/utc prior to merging changes affecting temporal rendering.
- Exception process: Submit an architecture review request detailing the justification for bypassing UTC normalization.
- Exception process: Document specific runtime boundaries and isolation guarantees preventing unnormalized timestamps from leaking to shared presentation layers.