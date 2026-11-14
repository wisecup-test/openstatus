---
version: 2
name: e28f24dd-4516-4656-8d6e-7108aed49a78-adoption-of-openstatus-db-src-schema-for-centralized-database-schema-definitions-domain-service-modules-not-import-internal
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Service Modules Not Import Internal"
metadata:
  actual:
    adr_id: e28f24dd-4516-4656-8d6e-7108aed49a78
    status: accepted
    candidate_group_id: ecbc9ccf-571a-40d2-96cf-eacf4af4f4d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Definitions: Domain Service Modules Not Import Internal

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/email`

Applies to:
- `src/**`

## Context

- Domain services such as notification dispatchers require strong contractual guarantees regarding database entity structures and constant definitions.
- Distributing database schema definitions across individual domain packages leads to schema divergence and fragile integration points between production logic and test mocks.
- The project maintains an internal workspace package @openstatus/db/src/schema that centralizes canonical data models and schema constants across services.

## Problem Statement

Without centralized schema definition governance, domain notification handlers and test mocks risk defining diverging data models, leading to silent runtime deserialization errors, inconsistent mock data, and high maintenance overhead when database contracts evolve.

## Decision

1. SHOULD_NOT: Domain service modules SHOULD_NOT import internal database connection or migration drivers directly when schema definitions alone satisfy typing and validation requirements.

## Policy Block

- SHOULD_NOT Domain service modules SHOULD_NOT import internal database connection or migration drivers directly when schema definitions alone satisfy typing and validation requirements.

In scope:
- Notification service packages handling database-backed event processing
- Test mock generation suites creating mock database entity representations

Out of scope:
- Stateless utility packages that operate without referencing database entities
- Client-side browser applications consuming public API contracts rather than internal database schemas

## Rationale

- Centralizing database schemas in @openstatus/db/src/schema establishes a single source of truth for entity types and constants across domain modules.
- Reusing canonical schemas in test mocks ensures that test data structures remain synchronized with production schema definitions as database entities evolve.
- Pairing centralized schema definitions with schema-driven validation guarantees safe payload parsing at domain service boundaries.

## Consequences

Positive:
- Eliminates schema drift across domain notification packages and test mock generation suites.
- Enforces consistent type contracts across production handlers and testing environments.
- Centralizes schema refactoring and migration impact within a single shared module.

Negative:
- Creates an explicit cross-package dependency between domain packages and the internal database schema package.
- Schema modifications in the central package require coordinated verification across all consuming domain packages.

## Alternatives

- Local schema redefinition within individual domain packages (rejected)
  Rejected because: Introduces significant risk of schema drift and runtime deserialization failures between services.
  When valid: Only when domain boundaries represent completely isolated microservices communicating exclusively over public transport protocols.
- Direct dependency on monolithic database persistence modules containing both drivers and schemas (rejected)
  Rejected because: Couples lightweight message processing and mock services to heavy database connection pools and runtime drivers.
  When valid: In monolithic service entrypoints that directly manage connection lifecycles.

## Risks

- Breaking changes to central schema contracts can disrupt downstream notification dispatchers and mock generation suites.
  Mitigation: Enforce automated workspace-wide typechecking and contract verification in continuous integration before publishing schema updates.
  Owner: Platform Engineering
- Deep internal submodule import paths may complicate package boundary enforcement.
  Mitigation: Expose clean public entrypoint exports from the database schema module to standardize import paths.
  Owner: Backend Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import schema objects and constants directly from the designated schema entrypoint of the internal database package.
- Use safe schema parsing functions to validate incoming notification payloads against imported schema definitions prior to executing business logic.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and type checking script from the repository manifest and execute it to verify all imports of @openstatus/db/src/schema resolve with complete type safety.
- Discover and execute the repository linting suite to ensure that no duplicate local database entity interfaces exist across domain notification packages.
- Discover and execute the automated test runner across notification packages to validate that mock suites and production handlers conform to shared schema definitions.

Accept when:
- All domain notification and test mock packages resolve database entity types and constants directly from @openstatus/db/src/schema without type errors.
- Static analysis confirms zero redundant database schema or entity definitions in domain notification directories.
- All unit and integration tests for notification processing and mock generation pass successfully.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace static analysis and linting.
- Verified by: Mandatory architectural peer review on all dependency and schema modification pull requests.
- Violation handling: Build pipeline failure on any unauthorized or redundant local schema declaration.
- Violation handling: Immediate pull request rejection until imports are aligned with the central schema module.
- Exception process: Formal review and approval by the platform engineering team documented in an architectural exception record.