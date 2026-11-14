---
version: 2
name: e9f61bf5-70dc-41a6-ad54-c0f9f0e14cc5-adoption-of-openstatus-db-src-schema-constants-for-centralized-domain-enumerations-application-components-data-utilities-not-declare
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Application Components Data Utilities Not Declare"
metadata:
  actual:
    adr_id: e9f61bf5-70dc-41a6-ad54-c0f9f0e14cc5
    status: accepted
    candidate_group_id: 845ebcd2-20cc-4558-8366-edf050da5222
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Application Components Data Utilities Not Declare

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`
- `src/data/**`

## Context

- Domain enumerations, scheduling configurations, and region definitions are referenced across user interface forms, visualization charts, and data access layers.
- Without a shared source of truth, schema constants duplicate across boundary layers, causing divergence when database values or periodicity intervals change.
- The internal module @openstatus/db/src/schema/constants provides canonical exports used across data fetchers and reactive client components.

## Problem Statement

Dispersed domain values and scheduling configurations across form validation schemas, metrics caching, and telemetry charts risk schema drift and type divergence between persistence models and consumer interfaces. Applications require a single authoritative location for domain-level constants and enumerations without redefining scalar intervals or status types in client components.

## Decision

1. MUST_NOT: Application components and data utilities MUST NOT declare local duplicate literals or re-export local shadows of constants provided by @openstatus/db/src/schema/constants.

## Policy Block

- MUST_NOT Application components and data utilities MUST NOT declare local duplicate literals or re-export local shadows of constants provided by @openstatus/db/src/schema/constants.

In scope:
- Validation schemas, data retrieval utilities, and user interface components referencing shared domain enumerations or database configuration constants.
- Application layers integrating with scheduling intervals, regional metadata, or database model definitions.

Out of scope:
- Component-internal state values and presentation flags that have no corresponding persistence or domain representation.
- Independent third-party utility configurations that do not interact with domain data contracts.

## Rationale

- Centralizing domain constants within @openstatus/db/src/schema/constants guarantees that client validation, reporting charts, and data fetchers remain strictly synchronized with database schema definitions.
- Evidence across five distinct data and presentation modules demonstrates repeated consumption of canonical exports, proving an existing architectural pattern of shared schema authority.
- Deriving validation schemas from shared constants eliminates runtime errors caused by mismatched enumerations or desynchronized periodic intervals.

## Consequences

Positive:
- Ensures single-source-of-truth consistency for domain enumerations across forms, charts, and backend queries.
- Reduces maintenance overhead by allowing database schema updates to propagate type and value changes directly to consumer validation rules.
- Prevents subtle data corruption and validation failures stemming from hardcoded string literals.

Negative:
- Couples frontend user interface and visualization components to the database package module structure.
- Increases client bundle surface when importing shared modules if tree-shaking is improperly configured across package boundaries.

## Alternatives

- Duplicate domain constants locally within individual application feature modules (rejected)
  Rejected because: Creating disconnected constant definitions leads to silent synchronization divergence between form validation and persistence models.
  When valid: Valid only in completely isolated throwaway prototypes or standalone scripts with no persistence dependencies.
- Expose domain constants through runtime HTTP configuration endpoints rather than shared build-time imports (rejected)
  Rejected because: Adds runtime latency, asynchronous loading states, and network failure modes for static domain enumerations.
  When valid: Valid when configuration values change dynamically at runtime without codebase redeployment.

## Risks

- Changes to exported constants in the schema module can cause breaking changes across dependent forms and data layers if updated without automated regression tests.
  Mitigation: Run cross-package type checks and test suites in automated continuous integration workflows prior to merging changes.
  Owner: engineering team
- Deep imports into internal package subpaths may break if the internal package exports structure changes.
  Mitigation: Maintain explicit package export maps and document supported public subpaths for consumer applications.
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
- Discover the workspace configuration to ensure path resolution maps the shared schema constants package correctly without circular dependencies.
- When constructing schema validation definitions, inspect exported constants to determine whether they are exposed as tuples, arrays, or object maps to ensure proper validator integration.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify no local duplicates of domain constants are defined in consumer modules.
- Discover and execute the type check and test suites across all workspace packages to verify imported constants satisfy interface contracts.

Accept when:
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace type verification and lint rules.
- Verified by: Peer code review auditing all additions of domain enumerations and validation schemas.
- Violation handling: Pull requests containing hardcoded duplicate domain strings or unapproved local constants must be blocked until refactored to use the shared constants module.
- Exception process: Exceptions require approval from the lead architect and must be documented in tracking tickets with technical rationale.