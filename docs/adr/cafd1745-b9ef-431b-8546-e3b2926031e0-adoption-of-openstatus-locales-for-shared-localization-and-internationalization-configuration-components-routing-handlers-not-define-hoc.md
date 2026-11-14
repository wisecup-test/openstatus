---
version: 2
name: cafd1745-b9ef-431b-8546-e3b2926031e0-adoption-of-openstatus-locales-for-shared-localization-and-internationalization-configuration-components-routing-handlers-not-define-hoc
description: "Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Routing Handlers Not Define Hoc"
metadata:
  actual:
    adr_id: cafd1745-b9ef-431b-8546-e3b2926031e0
    status: accepted
    candidate_group_id: afff3de8-6b16-4c07-818e-ae7a6482e6ed
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
      - src/content/markdown/**
---

# Adoption of @openstatus/locales for Shared Localization and Internationalization Configuration: Components Routing Handlers Not Define Hoc

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/**`
- `src/content/markdown/**`

## Context

- Application features spanning route matching and user interface presentation require localized string resolution and date formatting.
- Static analysis demonstrates that @openstatus/locales is imported across routing handlers and client UI components to supply shared locale configurations.
- A centralized shared package is required to avoid diverging locale mappings and redundant internationalization setups across workspace consumers.

## Problem Statement

Handling internationalization and localized date rendering across separate application concerns without a single source of truth leads to inconsistent locale support, redundant configuration objects, and disjointed route matching behaviors.

## Decision

1. MUST_NOT: Components and routing handlers must not define ad-hoc locale lists, hardcoded language constants, or local date formatting locale mappings outside the @openstatus/locales package.

## Policy Block

- MUST_NOT Components and routing handlers must not define ad-hoc locale lists, hardcoded language constants, or local date formatting locale mappings outside the @openstatus/locales package.

In scope:
- Application routing logic requiring localized route matching and locale resolution.
- UI components and context providers requiring localized date formatting or calendar displays.

Out of scope:
- Non-localized background scripts and system tasks that do not interact with user locale configurations.

## Rationale

- Evidence from the codebase shows @openstatus/locales utilized across both content routing utilities and client-rendered date formatting providers.
- Consolidating locale definitions within @openstatus/locales establishes a clear single source of truth for all locale identifiers, routing constraints, and formatting providers.
- Decoupling specific UI and routing components from third-party date or i18n configurations allows internal locale rules to evolve independently of downstream consumers.

## Consequences

Positive:
- Centralizes application locale definitions, ensuring consistent route matching and user interface presentation.
- Eliminates duplicated locale dictionaries and date formatting mappings across independent modules.
- Simplifies adding or modifying supported application locales by localizing changes to a single package boundary.

Negative:
- Consumer modules must take a dependency on the internal shared package, requiring workspace dependency coordination.
- Changes to locale definitions or formatting adapters necessitate interface alignment across both server routing and client UI components.

## Alternatives

- Decentralized in-component locale declarations and independent third-party date locale imports (rejected)
  Rejected because: Duplicating locale lists and configurations across routing modules and UI components risks translation discrepancies, inconsistent date formatting, and increased bundle duplication.
  When valid: Standalone prototypes or single-file scripts that do not integrate with the broader application routing and UI ecosystem.
- Direct third-party localization library configuration without an internal abstraction package (rejected)
  Rejected because: Couples every routing and UI component directly to third-party library internals rather than maintaining a unified domain-specific locale interface.
  When valid: Small monolithic applications with no workspace modularity or shared package boundaries.

## Risks

- Breaking contract changes in @openstatus/locales can simultaneously break route matching and client UI date rendering.
  Mitigation: Ensure comprehensive type tests and schema validation exist within @openstatus/locales to catch breaking changes before publishing to consumer packages.
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
- Inspect the workspace package dependency configurations to ensure clean dependency graphs between application components and @openstatus/locales.
- Ensure client context providers wrapping date libraries standardize their locale inputs via @openstatus/locales exports.

## Continuation Context


Verify commands:
- Discover and run the project static type checker across workspace packages to verify locale import compatibility.
- Discover and execute the project linter and dependency boundary checks to confirm all locale references route through the shared locale package.
- Discover and run the workspace test suite to validate locale resolution and date formatting behavior.

Accept when:
- The project static analysis and type checking suite passes with zero errors referencing @openstatus/locales imports.
- All route matching and date provider configurations resolve locale definitions from @openstatus/locales without duplicate locale definitions.

## Enforcement

- Verified by: Automated pull request dependency analysis and workspace linter boundaries.
- Verified by: Static type checking during continuous integration builds.
- Verified by: Peer architectural review on pull requests touching internationalization and locale configurations.
- Violation handling: Automated continuous integration failures when unapproved locale definitions or duplicate locale libraries are introduced.
- Violation handling: Code review blocks requiring refactoring to consume @openstatus/locales.
- Exception process: Submit an architectural change request detailing why the localized functionality cannot be provided by @openstatus/locales.
- Exception process: Obtain approval from the frontend platform leads and update workspace documentation accordingly.