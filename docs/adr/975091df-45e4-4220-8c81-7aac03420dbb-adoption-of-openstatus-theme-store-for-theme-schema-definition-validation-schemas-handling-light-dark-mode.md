---
version: 2
name: 975091df-45e4-4220-8c81-7aac03420dbb-adoption-of-openstatus-theme-store-for-theme-schema-definition-validation-schemas-handling-light-dark-mode
description: "Adoption of @openstatus/theme-store for Theme Schema Definition: Validation Schemas Handling Light Dark Mode"
metadata:
  actual:
    adr_id: 975091df-45e4-4220-8c81-7aac03420dbb
    status: accepted
    candidate_group_id: 953354fc-1228-4815-8dc1-ee03dbe3f2ef
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/pages/**
---

# Adoption of @openstatus/theme-store for Theme Schema Definition: Validation Schemas Handling Light Dark Mode

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/pages/**`

## Context

- Database persistence schemas and API validation layers require precise definitions for configurable theme variables such as light and dark mode properties.
- Multiple modules across the repository handle theme structures, introducing the risk of schema drift when definitions are duplicated locally.
- The internal module @openstatus/theme-store provides shared theme definitions and validation primitives that can be reused across service and persistence boundaries.

## Problem Statement

Defining page-level visual theme configurations across multiple persistence and validation files without a shared contract leads to redundant schema declarations, potential drift between storage and display layers, and fragile runtime validation.

## Decision

1. MUST: Validation schemas handling light and dark mode theme variables MUST compose themeVarsSchema from the shared module to ensure structural consistency.

## Policy Block

- MUST Validation schemas handling light and dark mode theme variables MUST compose themeVarsSchema from the shared module to ensure structural consistency.

In scope:
- Database schema definitions and validation layers that persist or validate theme configurations.
- Internal packages that define entities configurable by light and dark mode styling variables.

Out of scope:
- Database entities and validation schemas that do not manage theme or display configuration.
- External service integrations with fixed third-party schemas unrelated to internal theme presentation.

## Rationale

- Static analysis confirms that page schema and validation modules consistently import @openstatus/theme-store to structure theme properties.
- Reusing theme contracts across database schemas and runtime validation models eliminates schema divergence between presentation and persistence boundaries.
- Centralizing theme definitions in an internal module guarantees consistent contract enforcement across repository packages.

## Consequences

Positive:
- Ensures a single source of truth for theme contracts and styling variables across data persistence and presentation layers.
- Prevents schema drift by validating theme objects against shared type definitions before persistence.
- Promotes reuse of validation logic across boundary layers and storage schemas.

Negative:
- Introduces a build-time and packaging dependency on the internal module across schema definition packages.
- Breaking changes to theme schemas require coordinated updates or version bumps across consuming modules.

## Alternatives

- Inlining theme validation structures directly within each data schema definition (rejected)
  Rejected because: Causes schema drift between data persistence layers and frontend presentation components, creating synchronization overhead and duplicate maintenance.
  When valid: Valid only in standalone prototypes or isolated micro-services with no shared presentation contracts.
- Sharing theme schemas through raw unstructured JSON fields without typed schema enforcement (rejected)
  Rejected because: Fails to prevent malformed theme configurations from entering persistent storage and removes compile-time type safety.
  When valid: Valid when storing arbitrary third-party payloads whose schema cannot be known at build time.

## Risks

- Changes in @openstatus/theme-store schemas may cause downstream validation failures across dependent persistence models.
  Mitigation: Enforce strict semantic versioning and regression testing for shared theme contracts across dependent packages.
  Owner: engineering team
- Tight coupling between data persistence packages and visual theme definitions could complicate module extraction or decoupling.
  Mitigation: Establish clear dependency boundaries and review module graph dependencies during code reviews.
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
- Import theme variable schemas from @openstatus/theme-store when constructing composite entity schemas requiring visual styling configuration.
- Ensure optional light and dark mode configuration properties reuse the shared theme variable schema without redefining nested property constraints.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify that theme references resolve to the shared module.
- Discover and execute test suites targeting database and validation schemas to confirm proper theme schema enforcement.
- Inspect the workspace package dependency graph to ensure no circular dependencies exist with the shared theme package.

Accept when:
- All schema and validation declarations requiring theme configuration import directly from @openstatus/theme-store.
- Repository static analysis and dependency graph audits confirm zero duplicate theme schema definitions across packages.
- Validation and schema tests verify that invalid theme variable structures are rejected at runtime.

## Enforcement

- Verified by: Automated module boundary linting and dependency tree verification during continuous integration.
- Verified by: Code review inspection verifying that schema definitions consume shared theme contracts rather than local structures.
- Verified by: Automated schema tests validating that theme configuration fields adhere to the shared validation contract.
- Violation handling: Pull requests introducing redundant theme schema structures will be blocked by automated checks.
- Violation handling: Violations discovered in active development must be refactored to import definitions from the shared module before merge approval.
- Exception process: Submit an architectural exception request detailing why the internal theme module cannot satisfy the specific schema requirements.
- Exception process: Obtain explicit sign-off from the platform architecture team before defining custom theme validation logic.
- Exception process: Document the exception with an expiration timeframe and a convergence migration path.