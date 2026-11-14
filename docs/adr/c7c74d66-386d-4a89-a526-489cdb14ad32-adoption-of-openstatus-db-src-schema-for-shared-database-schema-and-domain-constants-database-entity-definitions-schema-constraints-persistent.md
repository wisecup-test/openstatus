---
version: 2
name: c7c74d66-386d-4a89-a526-489cdb14ad32-adoption-of-openstatus-db-src-schema-for-shared-database-schema-and-domain-constants-database-entity-definitions-schema-constraints-persistent
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Database Entity Definitions Schema Constraints Persistent"
metadata:
  actual:
    adr_id: c7c74d66-386d-4a89-a526-489cdb14ad32
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Database Entity Definitions Schema Constraints Persistent

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/**`

## Context

- Domain models and database entity definitions must coordinate across multiple application tiers including client components and server endpoints.
- Disparate implementations of domain enums across forms and presentation views introduce the risk of schema drift and contract mismatches.
- A centralized schema module within the workspace provides uniform entity contracts and domain constants across consuming packages.

## Problem Statement

Decentralized schema declarations and redundant domain enum definitions across client forms, presentation tables, and server endpoints cause schema drift and brittle runtime contracts. A standardized shared module is required to maintain a single source of truth for all database entity structures and enumeration values across tiers.

## Decision

1. MUST: All database entity definitions, schema constraints, and persistent domain enumeration constants MUST be sourced directly from the shared @openstatus/db/src/schema module.

## Policy Block

- MUST All database entity definitions, schema constraints, and persistent domain enumeration constants MUST be sourced directly from the shared @openstatus/db/src/schema module.

In scope:
- Any application tier defining or validating database entity structures.
- Client-side form controllers, data table components, and backend route handlers interacting with domain entities.

Out of scope:
- Pure presentation styling components with no domain data validation or entity interaction.
- External third-party integration points with independent contract schemas.

## Rationale

- Imports across twenty-four files demonstrate centralized reliance on @openstatus/db/src/schema for data schemas and enumeration values.
- Centralizing database schemas in a dedicated module ensures end-to-end type safety between data access patterns and presentation components.
- Shared enum constants prevent divergence between form validation boundaries and backend persistence logic.

## Consequences

Positive:
- Ensures consistent entity types and enumeration constants across client and server execution boundaries.
- Eliminates data validation inconsistencies between user input forms and persistence structures.
- Reduces maintenance overhead by centralizing schema definitions in a single authoritative module.

Negative:
- Creates an architectural dependency coupling between client-side presentation components and database schema definitions.
- Schema modifications require coordinated verification across all dependent client forms and presentation tables.

## Alternatives

- Duplicating entity types and enum definitions independently within each client component and server route (rejected)
  Rejected because: Leads to schema drift, elevated maintenance overhead, and silent runtime validation errors.
  When valid: Valid only in decoupled polyglot services where shared workspace modules cannot be imported.
- Generating standalone client-side contract schemas from an external serialization specification (rejected)
  Rejected because: Adds unnecessary build orchestration complexity when a shared internal module natively provides typed schemas.
  When valid: Valid when external consumers lack access to internal workspace packages.

## Risks

- Client bundles could inadvertently import server-specific database runtime code if module exports are improperly structured.
  Mitigation: Strictly isolate schema definitions and constants from runtime database connection logic within the shared package.
  Owner: engineering team
- Breaking changes to schema exports immediately affect both frontend forms and backend handlers.
  Mitigation: Use automated type validation and regression test suites across all packages prior to merging schema modifications.
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
- Import entity schemas and domain enumeration constants strictly from the designated shared module subpath rather than instantiating duplicate local structures.
- Ensure that client components import only static schema definitions and type utilities to prevent bundling runtime database drivers.

## Continuation Context


Verify commands:
- Discover and run the project static type analysis script to verify all schema imports and type usages across the workspace.
- Discover and execute the workspace test suite to confirm that validation rules and domain enums match expected behavior across all consuming modules.

Accept when:
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

## Enforcement

- Verified by: Continuous integration pipeline static analysis and type verification checks.
- Verified by: Peer code review auditing all new entity references to ensure consumption from the shared schema module.
- Violation handling: Continuous integration builds fail upon detecting local enum duplication or broken schema type contracts.
- Violation handling: Code review changes requested when entity structures are defined outside the designated shared module.
- Exception process: Exceptions require documented architectural justification approved by principal engineering before local schema overrides are merged.