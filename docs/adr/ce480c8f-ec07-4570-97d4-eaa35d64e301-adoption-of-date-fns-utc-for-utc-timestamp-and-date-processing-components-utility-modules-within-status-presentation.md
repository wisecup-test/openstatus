---
version: 2
name: ce480c8f-ec07-4570-97d4-eaa35d64e301-adoption-of-date-fns-utc-for-utc-timestamp-and-date-processing-components-utility-modules-within-status-presentation
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Components Utility Modules Within Status Presentation"
metadata:
  actual:
    adr_id: ce480c8f-ec07-4570-97d4-eaa35d64e301
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Components Utility Modules Within Status Presentation

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- Timestamp rendering and date calculations within status monitoring and reporting blocks require consistent, timezone-agnostic processing.
- Client-side execution environments introduce local timezone variances that can distort status duration calculations and display inaccurate historical incident times.
- Native date constructs lack predictable UTC context encapsulation, necessitating a specialized UTC date handling extension integrated with standard date processing functions.
- The user interface components and formatting utility modules require an explicit, standardized dependency for all UTC temporal conversions and displays.

## Problem Statement

Status tracking components and temporal utility functions must compute and present incident timestamps consistently across diverse client environments without variance caused by local browser timezones. Using uncoordinated date primitives or inconsistent timezone handling leads to discrepancies in reported status windows, inaccurate uptime metrics, and client-side hydration mismatches.

## Decision

1. MUST_NOT: Components and utility modules within the status presentation layer MUST NOT invoke native uncoordinated local time methods or introduce untracked local timezone offsets for status timestamps.

## Policy Block

- MUST_NOT Components and utility modules within the status presentation layer MUST NOT invoke native uncoordinated local time methods or introduce untracked local timezone offsets for status timestamps.

In scope:
- Status monitoring blocks, timestamp presentation components, and temporal formatting utility functions.
- Any frontend module calculating duration intervals, incident timestamps, or uptime status metrics.

Out of scope:
- General application features with purely local user calendar preferences that explicitly require local timezone display.
- Backend persistence services that manage temporal data via database native timestamp types.

## Rationale

- @date-fns/utc provides functional, immutable UTC date constructs that seamlessly interoperate with date-fns while preventing client-side timezone drift.
- Standardizing on a dedicated UTC extension ensures uniform rendering of status intervals across globally distributed users and avoids client-side hydration mismatches.
- Centralizing date normalization logic within utility functions and shared timestamp blocks reduces duplication and eliminates bespoke timezone offset calculations.

## Consequences

Positive:
- Guarantees deterministic, timezone-invariant status event rendering across disparate client runtime environments.
- Maintains composability with existing date manipulation utilities through lightweight functional primitives.
- Prevents client-side hydration mismatches caused by server-to-client timezone divergence.

Negative:
- Developers must explicitly construct dates within UTC contexts rather than relying on standard date instantiation.
- Introduces an additional companion package dependency that must be maintained and kept compatible with the primary date manipulation library.

## Alternatives

- Native date primitives with manual offset arithmetic (rejected)
  Rejected because: Manual offset calculations are error-prone, susceptible to daylight saving anomalies, and lack declarative formatting integration.
  When valid: When operating in constrained environments where external dependencies are strictly prohibited.
- Comprehensive multi-timezone internationalization libraries (rejected)
  Rejected because: Monolithic timezone libraries introduce excessive bundle overhead and complex timezone database loading when only UTC normalization is required.
  When valid: When an application explicitly requires multi-timezone conversions across diverse geographical locales.

## Risks

- Developers may inadvertently use native date instantiation or local formatting methods, bypassing UTC normalization.
  Mitigation: Establish code review checks and static analysis rules to flag direct local date calls in status timestamp components.
  Owner: Frontend Engineering Team
- Version drift or API incompatibility between the companion UTC library and the base date processing library.
  Mitigation: Adhere to the Discovery Policy lock-version grounding to confirm compatible peer dependency versions before updating dependencies.
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
- Construct status timestamp dates using UTC wrappers provided by @date-fns/utc before passing them into formatting utilities or UI presentation hooks.
- Ensure all shared formatting utility functions expose UTC-normalized interfaces to downstream status block components.

## Continuation Context


Verify commands:
- Discover the project test execution script from the repository manifest and execute it against status block utilities to verify UTC timestamp calculations.
- Discover the project static analysis and linting script from the repository manifest and execute it to verify dependency imports and type compliance across UI components.

Accept when:
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

## Enforcement

- Verified by: Automated continuous integration pipelines executing test suites across diverse timezone environments.
- Verified by: Peer code review verification of temporal calculations and component timestamp rendering.
- Violation handling: Pull requests introducing uncoordinated date instantiation or unapproved date libraries in status blocks are blocked pending refactoring.
- Exception process: Exceptions for local-time display must be submitted with architectural review and explicit justification to the frontend architecture team.