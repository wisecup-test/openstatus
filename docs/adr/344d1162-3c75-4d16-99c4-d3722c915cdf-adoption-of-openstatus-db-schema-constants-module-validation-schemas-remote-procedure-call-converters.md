---
version: 2
name: 344d1162-3c75-4d16-99c4-d3722c915cdf-adoption-of-openstatus-db-schema-constants-module-validation-schemas-remote-procedure-call-converters
description: "Adoption of @openstatus/db Schema Constants Module: Validation Schemas Remote Procedure Call Converters"
metadata:
  actual:
    adr_id: 344d1162-3c75-4d16-99c4-d3722c915cdf
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Validation Schemas Remote Procedure Call Converters

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- The server codebase implements API routing, environment configuration, and remote procedure call handlers that require domain validation, enum mappings, and limit enforcement.
- Five distinct modules across server route handlers, environment definitions, schema declarations, and RPC converters import domain constants directly from @openstatus/db/src/schema/constants.
- Maintaining consistent constraints between database schemas and API boundaries requires a canonical source of truth for domain limits and enum values.

## Problem Statement

Decentralized declaration of domain constants, validation limits, and enumeration values across API routes and RPC handlers creates risks of schema divergence, validation bypasses, and maintenance overhead when domain models change.

## Decision

1. SHOULD: Validation schemas and remote procedure call converters SHOULD directly reference exported constants from @openstatus/db/src/schema/constants when constructing schema rules and type transformations.

## Policy Block

- SHOULD Validation schemas and remote procedure call converters SHOULD directly reference exported constants from @openstatus/db/src/schema/constants when constructing schema rules and type transformations.

In scope:
- Server routing modules, remote procedure call handlers, and validation schemas that interact with domain constants or database entities
- Environment configuration and converter modules requiring canonical schema limits or enums

Out of scope:
- Isolated client-side presentation utilities that have no interaction with database schema constants or server domain models

## Rationale

- Centralizing schema constants and domain limits within @openstatus/db establishes a single source of truth across the monorepo, eliminating drift between database definitions and application route handlers.
- Reusing canonical constants across remote procedure call converters and validation schemas prevents duplication and guarantees consistent validation logic across all communication protocols.
- Consuming database schema constants from a shared internal package maintains architectural consistency and reduces maintenance overhead when schema definitions evolve.

## Consequences

Positive:
- Guarantees data consistency between database schema definitions and API input validation layers.
- Prevents constant divergence and silent drift across disparate communication handlers and converters.
- Reduces redundant declarations and simplifies maintenance when domain limits or enums change.

Negative:
- Couples service validation logic and converters to the internal database schema package structure.
- Changes to shared schema constants require coordination and validation across all consuming modules.

## Alternatives

- Independent local constant definitions within individual service handlers and route files (rejected)
  Rejected because: Leads to schema divergence, duplication, and discrepancies between database constraints and runtime validation rules
  When valid: In decoupled microservices where cross-package code sharing is prohibited
- Extracting domain constants into a standalone protocol-agnostic package separate from database schemas (deferred)
  Rejected because: Current architecture couples database schemas and domain constants within @openstatus/db, and extracting an additional package introduces unnecessary indirection
  When valid: When client applications or third-party consumers require domain constants without taking a dependency on database schema modules

## Risks

- Breaking changes in shared database constants can unintentionally invalidate downstream validation schemas or converter logic
  Mitigation: Enforce automated type checking and regression test suites across all consuming packages within the continuous integration pipeline
  Owner: Engineering Team
- Deep imports into internal package structures may break if the internal layout of @openstatus/db is refactored
  Mitigation: Establish stable package export entry points for schema constants and deprecate direct paths to internal source directories
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When introducing new domain constants, declare them alongside corresponding schema entities within @openstatus/db and export them via the schema constants module.
- Ensure that input validation schemas and remote procedure call converters consume exported constants rather than hardcoded primitive values.

## Continuation Context


Verify commands:
- Discover the primary package configuration script for static type verification and execute it.
- Discover the workspace test execution script and run the automated test suite against consuming packages.
- Inspect the repository script definitions and execute the linter to verify import boundary constraints.

Accept when:
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository verification scripts.
- Verified by: Peer code review verifying adherence to centralized schema constant imports.
- Violation handling: Pull requests containing duplicated domain constants or unapproved module imports will fail automated validation.
- Violation handling: Engineers must replace duplicated constants with centralized imports from @openstatus/db/src/schema/constants before merging.
- Exception process: Submit an architectural review request detailing why a local constant definition is strictly required.
- Exception process: Obtain explicit sign-off from the technical lead prior to introducing standalone constants outside the shared database schema package.