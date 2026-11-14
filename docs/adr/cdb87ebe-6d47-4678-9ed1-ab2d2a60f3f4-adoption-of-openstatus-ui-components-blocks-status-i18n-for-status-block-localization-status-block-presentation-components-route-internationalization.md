---
version: 2
name: cdb87ebe-6d47-4678-9ed1-ab2d2a60f3f4-adoption-of-openstatus-ui-components-blocks-status-i18n-for-status-block-localization-status-block-presentation-components-route-internationalization
description: "Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Presentation Components Route Internationalization"
metadata:
  actual:
    adr_id: cdb87ebe-6d47-4678-9ed1-ab2d2a60f3f4
    status: accepted
    candidate_group_id: b9b70b53-2d67-492e-a095-8040301ca6c7
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status-i18n for Status Block Localization: Status Block Presentation Components Route Internationalization

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- Status page user interfaces comprise multiple distinct visual blocks such as calendars, feeds, update forms, and banners that display user-facing text and timestamps
- Maintaining consistent terminology and supporting internationalization across status components requires a standardized localization strategy
- Static analysis across status component implementations demonstrates uniform dependency on @openstatus/ui/components/blocks/status-i18n

## Problem Statement

Status page blocks require consistent user-facing messaging, timestamps, and status labels across multiple disparate UI components. Without a governed module for internationalization, components risk diverging in terminology, embedding hardcoded string literals, and complicating multi-language support.

## Decision

1. MUST: Status block presentation components MUST route all internationalization, localized strings, and text translations through @openstatus/ui/components/blocks/status-i18n.

## Policy Block

- MUST Status block presentation components MUST route all internationalization, localized strings, and text translations through @openstatus/ui/components/blocks/status-i18n.

In scope:
- Status block UI components and status utility modules rendering user-facing text or formatted dates

Out of scope:
- Low-level headless primitive components devoid of user-facing text
- Backend services and data processing pipelines independent of status UI presentation

## Rationale

- Twelve separate status block component modules consistently consume @openstatus/ui/components/blocks/status-i18n, establishing an architectural consensus on centralized block localization
- Centralizing localization prevents string drift and ensures status terminology remains uniform across feeds, calendars, banners, and update controls
- Abstracting translation dictionaries into a dedicated module isolates visual presentation from localization loading and formatting mechanics

## Consequences

Positive:
- Enforces uniform terminology and translations across all status page visual blocks and notification feeds
- Decouples visual block presentation structure from locale data and translation dictionary implementations
- Simplifies future additions of localized languages through a single centralized module interface

Negative:
- Modifying or adding localized labels requires updating the central localization dictionary rather than editing component templates in place
- Couples all status block visual components to the shared internal localization module interface

## Alternatives

- Direct inline string literals within component presentation markup (rejected)
  Rejected because: Scatters localized text across multiple presentation files, prevents runtime locale switching, and duplicates common status terminology
  When valid: Prototyping temporary mock components before localization dictionaries are established
- Independent third-party localization libraries imported per component (rejected)
  Rejected because: Introduces fragmented configuration, duplicate translation bundles, and inconsistent fallback semantics across status page blocks
  When valid: Isolated standalone micro-applications with independent deployment lifecycles

## Risks

- Breaking interface changes in @openstatus/ui/components/blocks/status-i18n could cause cascading compile-time or runtime failures across all status blocks
  Mitigation: Provide comprehensive type definitions and unit tests within the localization module to verify translation completeness and key stability
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
- Coordinate all component string keys and formatting parameters with the interface exposed by the status-i18n module
- Maintain type definitions for translation keys to ensure compile-time verification across all referencing status blocks

## Continuation Context


Verify commands:
- Discover and execute repository linting and static analysis scripts to verify that status block components import localization resources exclusively through @openstatus/ui/components/blocks/status-i18n.
- Discover and run module dependency boundary validation checks to ensure no hardcoded string literals or unauthorized localization imports exist in status block components.

Accept when:
- All status block presentation components source localized strings exclusively from @openstatus/ui/components/blocks/status-i18n
- Static analysis and verification suites pass without detecting inline hardcoded text literals or unauthorized localization imports in status block components
- Dependency boundary verification confirms zero unapproved localization dependencies imported across block components

## Enforcement

- Verified by: Automated linting rules verifying imports within status block component directories
- Verified by: Continuous integration verification suites checking module dependency boundaries
- Verified by: Peer review during code integration
- Violation handling: Continuous integration failures on unauthorized localization imports or hardcoded user-facing strings
- Violation handling: Mandatory remediation to route string resolution through @openstatus/ui/components/blocks/status-i18n prior to merging
- Exception process: Submit an architecture exception request documenting why the centralized localization module cannot satisfy the presentation requirements
- Exception process: Obtain formal sign-off from the front-end architecture review team prior to merging any non-standard localization implementation