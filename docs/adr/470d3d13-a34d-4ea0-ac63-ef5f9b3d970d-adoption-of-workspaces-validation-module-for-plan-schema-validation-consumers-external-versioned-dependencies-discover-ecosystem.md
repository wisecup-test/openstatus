---
version: 2
name: 470d3d13-a34d-4ea0-ac63-ef5f9b3d970d-adoption-of-workspaces-validation-module-for-plan-schema-validation-consumers-external-versioned-dependencies-discover-ecosystem
description: "Adoption of workspaces/validation Module for Plan Schema Validation: Consumers External Versioned Dependencies Discover Ecosystem"
metadata:
  actual:
    adr_id: 470d3d13-a34d-4ea0-ac63-ef5f9b3d970d
    status: accepted
    candidate_group_id: f010b973-6aa7-44f9-9802-0a4ddfc028ad
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/plan/**
---

# Adoption of workspaces/validation Module for Plan Schema Validation: Consumers External Versioned Dependencies Discover Ecosystem

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/plan/**`

## Context

- Database plan configurations and schema utilities require consistent validation of plan limits to avoid corrupt or invalid state.
- Disparate modules managing plan definitions previously risked implementing divergent limit rules and ad-hoc validation logic.
- The codebase establishes a shared workspaces/validation module containing limitsSchema to provide a centralized contract for plan validations.

## Problem Statement

Managing plan configurations across database schema definitions without centralized validation creates risk of constraint divergence, invalid limit assignments, and inconsistent runtime errors at domain boundaries.

## Decision

1. MUST: Consumers of external or versioned dependencies MUST discover the ecosystem lock file and verify the resolved dependency version before implementing schema validation bindings.

## Policy Block

- MUST Consumers of external or versioned dependencies MUST discover the ecosystem lock file and verify the resolved dependency version before implementing schema validation bindings.

In scope:
- All database schema modules defining plan limits, configuration entities, and related utility operations.
- Any service or domain layer modifying plan limits and workspace configurations.

Out of scope:
- Third-party boundary payloads already validated by external API gateway schemas.
- Raw database query builders and migrations that do not handle domain entity configuration.

## Rationale

- Adopting the shared workspaces/validation module standardizes boundary validation across database plan configurations.
- Utilizing limitsSchema.parse provides fail-fast validation guarantees, ensuring only valid plan limits enter the database layer.
- Eliminating duplicate validation schemas ensures uniform evolution of domain rules across the entire workspace.

## Consequences

Positive:
- Centralized schema validation prevents configuration drift across plan limit definitions.
- Runtime boundaries fail fast with typed schema errors when invalid limits are provided.
- Schema updates in the shared validation module propagate automatically across all consumer modules.

Negative:
- Modules importing shared validation logic become coupled to the schema definitions of the workspace validation module.
- Executing schema parsing introduces runtime computational overhead during configuration evaluation.

## Alternatives

- Inline custom assertions without shared validation schemas (rejected)
  Rejected because: Duplicating assertion logic across configuration modules leads to drift in limit constraints and inconsistent validation error formats.
  When valid: Prototyping standalone utilities that operate without shared domain models.
- Database-level check constraints without application-layer schema parsing (rejected)
  Rejected because: Defers validation errors to database transactions, preventing early failure and clear error reporting at the application boundary.
  When valid: Environments where database triggers enforce all cross-table integrity rules uniformly.

## Risks

- Coupling plan configuration definitions to workspace validation modules may introduce circular import cycles if dependencies are not strictly layered.
  Mitigation: Maintain strict unidirectional imports from domain schemas to shared validation definitions.
  Owner: Database engineering team
- Uncaught parse errors from schema validation could trigger unhandled exceptions during plan configuration evaluation.
  Mitigation: Enforce defensive validation handling and propagate typed validation errors up the call stack.
  Owner: Backend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the shared validation exports in the repository and import schema validation contracts directly into domain configuration modules.
- Ensure schema parse operations occur before domain data structures are passed to downstream persistence or utility handlers.

## Continuation Context


Verify commands:
- Discover and run the project static type checker and linter scripts configured in the repository manifest.
- Execute the repository test suite targeted at schema configuration and validation utilities.

Accept when:
- Static type analysis and schema validation tests compile and execute without errors.
- Test suites asserting valid and invalid plan limit structures pass successfully against the validation contract.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository test and lint checks.
- Verified by: Peer code reviews verifying compliance with shared schema import standards.
- Violation handling: Pull requests containing inline limit checks or bypassing schema parsing will be blocked from merging.
- Exception process: Exceptions must be submitted via architectural review and approved by engineering leads.