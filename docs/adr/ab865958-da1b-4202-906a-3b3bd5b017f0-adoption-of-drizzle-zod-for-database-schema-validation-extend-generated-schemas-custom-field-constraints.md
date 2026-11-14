---
version: 2
name: ab865958-da1b-4202-906a-3b3bd5b017f0-adoption-of-drizzle-zod-for-database-schema-validation-extend-generated-schemas-custom-field-constraints
description: "Adoption of drizzle-zod for Database Schema Validation: Extend Generated Schemas Custom Field Constraints"
metadata:
  actual:
    adr_id: ab865958-da1b-4202-906a-3b3bd5b017f0
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Extend Generated Schemas Custom Field Constraints

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- The persistence architecture defines relational tables within schema modules while requiring strict runtime validation for incoming API inputs, workspace limits, and configuration objects.
- Maintaining independent manual type definitions for relational persistence and request payload validation introduces schema drift and duplicate maintenance overhead.
- Schema validation modules co-locate table definitions with schema generator utilities to ensure that data transfer validation schemas stay synchronized with underlying table structures.

## Problem Statement

Application boundaries require strict runtime input validation before mutating persisted state, but manually maintaining separate validation models alongside relational table definitions creates duplicate maintenance overhead and introduces schema drift when underlying tables change.

## Decision

1. SHOULD: SHOULD extend generated schemas with custom field constraints, default values, and transformations when payload specifications diverge from raw table types.

## Policy Block

- SHOULD SHOULD extend generated schemas with custom field constraints, default values, and transformations when payload specifications diverge from raw table types.

In scope:
- All database schema definitions and data validation modules defining entity constraints.
- Ingestion boundaries handling API payloads destined for database persistence.

Out of scope:
- Ephemeral client-only UI state models that do not persist to database tables.
- External third-party API response shapes that do not map to internal database schemas.

Exceptions:
- EXC-20-001: Payloads require dynamic or unstructured JSON representations that cannot be modeled by relational table definitions.

## Rationale

- Deriving validation schemas directly from database tables enforces a single source of truth for column constraints, nullability, and primary types across persistence and transport layers.
- Co-locating validation schemas with table definitions prevents drift between database migrations and request validation logic.
- Extending base generated schemas with property validators enables fine-grained domain constraints without breaking alignment with storage types.

## Consequences

Positive:
- Guarantees schema synchronization between database table columns and ingestion validation contracts.
- Reduces boilerplate code by automatically generating baseline validation models for table inserts and queries.
- Centralizes schema modifications so database table updates automatically propagate to validation checks.

Negative:
- Tight coupling between the persistence layer structure and public API transport contracts.
- Increased maintenance complexity from managing peer dependency resolution between the ORM and the schema derivation library.
- Requires explicit overrides when public API inputs intentionally diverge from underlying relational column schemas.

## Alternatives

- Handcrafted standalone schema definitions independent of database tables (rejected)
  Rejected because: Leads to schema drift, duplicate maintenance overhead, and silent contract divergence during database migrations
  When valid: Valid for isolated services or standalone consumers that have no direct access to database definitions
- Database-level constraint validation only without application-level runtime parsing (rejected)
  Rejected because: Results in raw database driver exceptions escaping to clients, unhelpful error responses, and delayed validation failures
  When valid: Valid for internal low-latency batch ingestion pipelines with pre-validated data streams

## Risks

- Table schema modifications may inadvertently expose internal column changes or alter public API validation contracts.
  Mitigation: Explicitly pick, omit, or extend generated schema fields rather than directly exposing raw generated table schemas to public routes.
  Owner: engineering team
- Version incompatibilities between the ORM core package and the schema derivation library causing build or runtime failures.
  Mitigation: Ground all dependency updates against repository lock artifacts and verify peer dependency ranges during updates.
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
- Derive baseline schemas using library generator functions and apply refinement methods to enforce business constraints on specific fields.
- Group table definitions and their associated validation schemas within the same domain directory to preserve structural co-location.

## Continuation Context


Verify commands:
- Discover and run the project test suite to verify that validation schemas correctly parse valid payloads and reject malformed inputs.
- Discover and execute the type-checking and schema validation scripts to confirm that derived schemas conform to database table types.

Accept when:
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

## Enforcement

- Verified by: Automated continuous integration test suites executing schema validation test cases.
- Verified by: Peer code review verifying that new database entities include corresponding derived validation schemas.
- Verified by: Static type analysis validating type synchronization between schema definitions and ingestion handlers.
- Violation handling: Pull requests lacking derived validation schemas for newly introduced database tables are blocked from merging.
- Violation handling: Runtime validation failures on unvalidated inputs trigger logged warnings and client-facing error responses.
- Exception process: Submit an architectural review request outlining the justification for custom uncoupled validation logic.
- Exception process: Obtain explicit sign-off from the technical lead and document the rationale in the corresponding validation module.