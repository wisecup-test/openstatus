---
version: 2
name: b0c31a8e-9d5a-4e06-87ea-3fcaab320d65-zod-schema-validation-for-database-query-result-hydration-engineers-verify-exact-resolved-version-zod
description: "Zod Schema Validation for Database Query Result Hydration: Engineers Verify Exact Resolved Version Zod"
metadata:
  actual:
    adr_id: b0c31a8e-9d5a-4e06-87ea-3fcaab320d65
    status: accepted
    candidate_group_id: a93d3679-6c55-490f-a8b5-255f0e5c5d25
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - script/**
---

# Zod Schema Validation for Database Query Result Hydration: Engineers Verify Exact Resolved Version Zod

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `script/**`

## Context

- Database scripts and migration routines execute select and update operations across persistent database tables.
- Raw query results returned from database drivers may contain unexpected null values, missing fields, or legacy data shapes that deviate from application expectations.
- Adopting runtime validation at the boundary of data access ensures data integrity before running transformations and updating persistent state.
- Zod provides declarative runtime validation schemas that parse raw query outputs and guarantee structural correctness.

## Problem Statement

Database data access and migration routines risk propagating malformed or invalid records into persistent storage when operating on unvalidated query results. Without runtime schema parsing at the point of query retrieval, structural anomalies in persisted records go undetected until mutation operations fail or corrupt downstream state.

## Decision

1. MUST: Engineers MUST verify the exact resolved version of Zod from the repository lock artifact and confirm API compatibility before implementing or modifying schema validation routines.

## Policy Block

- MUST Engineers MUST verify the exact resolved version of Zod from the repository lock artifact and confirm API compatibility before implementing or modifying schema validation routines.

In scope:
- Database migration scripts and data access routines that retrieve and transform persisted records

Out of scope:
- Stateless read-only utilities that do not execute database mutations or handle raw persisted data
- Direct database schema definition files and table constant declarations

## Rationale

- Runtime validation via Zod parse methods intercepts corrupted or unexpected database records before transformation logic executes.
- Pairing database select operations with array schema parsing enforces type safety at the data access boundary without relying on blind compile-time type assertions.
- Validating before updating ensures that database update mutations only persist verified records back to the database.

## Consequences

Positive:
- Ensures data integrity during database migration and batch update operations by catching schema deviations early.
- Provides typed and runtime-validated records for all downstream data manipulation and update logic.
- Prevents partial or corrupted data migrations from writing invalid states into persistent tables.

Negative:
- Introduces CPU parsing overhead for large result sets during batch data retrieval.
- Requires maintaining synchronized Zod validation schemas alongside evolving database table schemas.

## Alternatives

- Unchecked type assertions on database query results (rejected)
  Rejected because: Compile-time type assertions provide zero runtime protection against legacy schema mismatches or corrupted table rows during migrations.
  When valid: Only in performance-critical read paths where data structures are already guaranteed by database constraints.
- Manual conditional property inspection and type guards (rejected)
  Rejected because: Manual property checks are verbose, error-prone, and difficult to keep consistent across multiple database access scripts.
  When valid: When inspecting single primitive fields without structured schema definitions.

## Risks

- Performance degradation when parsing very large database result sets through schema validation.
  Mitigation: Chunk query results in batches during migrations and apply targeted selection schemas that only validate modified fields.
  Owner: Engineering Team
- Schema drift between database table definitions and corresponding Zod validation schemas.
  Mitigation: Co-locate selection schemas with database table definitions and enforce synchronization through automated validation tests.
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
- Discover the database table schemas and corresponding validation schemas from the database package source directories before authoring migration scripts.
- Execute data migration routines in transactional chunks to ensure atomicity when parsing query outputs and applying update mutations.

## Continuation Context


Verify commands:
- Discover and run the project static type checker to verify that all database query results pass through schema parse methods before mutation.
- Discover and run the project test suite to validate that data access and migration routines reject invalid record shapes during schema parsing.

Accept when:
- All database query results in migration scripts are parsed through Zod schemas prior to data transformation.
- The project type verification and test suites pass without schema parsing or type errors.

## Enforcement

- Verified by: Automated test execution in continuous integration verifying that schema validation failures halt migration execution.
- Verified by: Code review inspection of all database scripts ensuring raw query outputs are parsed before update operations.
- Violation handling: Pull requests containing unvalidated database query results or raw type casts on database entities are blocked until schema parsing is added.
- Exception process: Exceptions require approval from the database architectural owner with documented performance justification for bypassing runtime validation.