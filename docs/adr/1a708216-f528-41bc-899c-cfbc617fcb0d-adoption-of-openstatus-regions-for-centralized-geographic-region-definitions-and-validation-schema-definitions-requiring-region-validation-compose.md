---
version: 2
name: 1a708216-f528-41bc-899c-cfbc617fcb0d-adoption-of-openstatus-regions-for-centralized-geographic-region-definitions-and-validation-schema-definitions-requiring-region-validation-compose
description: "Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Schema Definitions Requiring Region Validation Compose"
metadata:
  actual:
    adr_id: 1a708216-f528-41bc-899c-cfbc617fcb0d
    status: accepted
    candidate_group_id: 12f4f803-fbbd-4c27-ad95-36c34f7a18c8
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/regions for Centralized Geographic Region Definitions and Validation: Schema Definitions Requiring Region Validation Compose

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- Database schema definitions and subscription plan configurations require validated geographic regions to enforce monitoring limits and multi-region availability.
- Multiple schema modules previously risked maintaining fragmented lists of supported regions and regional defaults.
- The codebase establishes @openstatus/regions as the authoritative module providing region schemas and standard location constants.

## Problem Statement

Decentralized or duplicate definitions of geographic regions across database schema definitions, plan configurations, and validation layers lead to configuration drift, invalid region assignments, and increased maintenance burden whenever supported infrastructure locations change.

## Decision

1. SHOULD: Schema definitions requiring region validation SHOULD compose validation primitives directly from the schemas exposed by @openstatus/regions.

## Policy Block

- SHOULD Schema definitions requiring region validation SHOULD compose validation primitives directly from the schemas exposed by @openstatus/regions.

In scope:
- Database schema modules defining plan limits, monitoring configurations, or regional infrastructure.
- Workspace and plan configuration modules validating geographic boundaries and regional defaults.

Out of scope:
- Transient network routing logic unrelated to persistent schema models and plan limits.
- Client-side localized display strings that do not represent discrete backend operational regions.

## Rationale

- Evidence across database schema, constants, and plan configuration files shows consistent adoption of @openstatus/regions for regional types and defaults.
- Centralizing region definitions avoids duplicating geographic enumerations across database tables, workspace validations, and subscription tiers.
- Composing schema validators directly from standard region schemas guarantees runtime inputs conform to supported infrastructure topologies.

## Consequences

Positive:
- Establishes a single source of truth for all geographic and monitoring region identifiers across schema definitions.
- Prevents drift between database constraints, workspace plan validation, and runtime region availability.
- Ensures regional default allocations in plan schemas are consistent and validated against centralized definitions.

Negative:
- Schema packages take a direct dependency on the centralized region module, requiring rebuilds when regions are updated.
- Adding new deployment or monitoring regions requires updating the centralized module before consuming schemas can reference them.

## Alternatives

- In-line hardcoded string arrays and local enumerations in schema definitions (rejected)
  Rejected because: Causes regional configuration drift, duplicate maintenance overhead, and schema inconsistency across workspace and plan boundaries.
  When valid: Prototyping isolated single-region features that will not be persisted or exposed to multi-region infrastructure.
- Dynamic runtime retrieval of active regions via external network service (rejected)
  Rejected because: Introduces external runtime network dependency and latency into static schema validation and build-time configuration generation.
  When valid: Systems where regional availability changes dynamically on sub-minute intervals without deployment cycles.

## Risks

- Changes to regional definitions in the upstream module may require coordinated updates across schema consumers.
  Mitigation: Enforce semantic versioning and verify downstream schema compatibility using automated test suites.
  Owner: Engineering Team
- Consumer divergence if individual modules attempt to extend regional schemas with ad-hoc overrides.
  Mitigation: Disallow local overrides through static analysis and pull request review checks.
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
- Import regional validation schemas and standard region constants directly from @openstatus/regions when defining plan boundaries or database schemas.
- Ensure default region arrays in plan configurations reference exported constants from the regional module rather than constructing literal arrays.

## Continuation Context


Verify commands:
- Discover the repository test runner from the project manifest and execute the test suite covering schema validation.
- Discover the static analysis validation command from the project manifest and run it to verify regional schema imports.
- Discover the module boundary verification script and execute it to ensure no undeclared region identifiers exist.

Accept when:
- All database schema and plan configuration tests pass with zero validation failures.
- Static analysis confirms all region references resolve directly to the centralized region module without raw string duplicates.
- Dependency checking verifies all schema definitions reference the locked version of the centralized region module.

## Enforcement

- Verified by: Continuous integration test pipelines validating schema serialization and plan parsing.
- Verified by: Static analysis checks detecting raw region string literals in schema definitions.
- Verified by: Peer code review for all modifications touching database schema and plan limits.
- Violation handling: Automated build failures on unauthorized local region definitions or invalid schema imports.
- Violation handling: Rejection of pull requests introducing unvalidated region string literals in database models.
- Exception process: Architectural review and written approval required for temporary non-standard region testing.