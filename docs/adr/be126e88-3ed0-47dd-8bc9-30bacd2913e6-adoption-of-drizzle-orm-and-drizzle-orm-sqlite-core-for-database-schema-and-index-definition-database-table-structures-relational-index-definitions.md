---
version: 2
name: be126e88-3ed0-47dd-8bc9-30bacd2913e6-adoption-of-drizzle-orm-and-drizzle-orm-sqlite-core-for-database-schema-and-index-definition-database-table-structures-relational-index-definitions
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Database Table Structures Relational Index Definitions"
metadata:
  actual:
    adr_id: be126e88-3ed0-47dd-8bc9-30bacd2913e6
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Database Table Structures Relational Index Definitions

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- The persistence layer requires consistent, type-safe definitions for relational database tables, unique constraints, and indexes.
- Static analysis of 33 schema definitions demonstrates universal usage of drizzle-orm and drizzle-orm/sqlite-core across persistence modules.
- Data models require explicit indexing on tenant identifiers, component relations, and timestamp columns to guarantee deterministic query latency.

## Problem Statement

Relational database schemas require static type verification and deterministic index configurations to eliminate runtime SQL errors, enforce relational integrity, and guarantee query performance across distributed service boundaries without maintaining disjoint SQL migration scripts.

## Decision

1. MUST_NOT: Database table structures and relational index definitions MUST_NOT be declared through unmanaged raw SQL statements when a corresponding declarative schema API is supported.

## Policy Block

- MUST_NOT Database table structures and relational index definitions MUST_NOT be declared through unmanaged raw SQL statements when a corresponding declarative schema API is supported.

In scope:
- All persistent relational entity schemas and table definitions across the database package.
- Index declarations, unique constraints, and relational foreign reference definitions for persistent data models.

Out of scope:
- In-memory transient data structures and cache models that do not persist to relational storage.
- Client-side presentation models operating outside the persistence layer.

## Rationale

- Evidence across 33 schema modules indicates deliberate, comprehensive standardization on drizzle-orm and drizzle-orm/sqlite-core for persistence modeling.
- Colocating index definitions directly with table declarations via builder APIs guarantees that performance constraints and uniqueness requirements are tracked alongside data structures.
- Static type generation prevents runtime mismatches between application models and underlying database schemas.

## Consequences

Positive:
- Guarantees compile-time type safety and automatic schema type inference across consuming application packages.
- Colocates relational table definitions, composite indexes, and uniqueness constraints within declarative TypeScript modules.
- Eliminates schema drift between runtime entity types and persistent relational table layouts.

Negative:
- Couples table model definitions to SQLite-specific dialect core modules, requiring dialect refactoring if storage engines change.
- Requires developers to learn library-specific schema builders and index configuration syntax rather than universal DDL.

## Alternatives

- Raw SQL migration scripts without declarative schema builders (rejected)
  Rejected because: Lacks compile-time TypeScript type safety and increases maintenance overhead across relational query boundaries.
  When valid: Valid when database features are entirely unsupported by declarative schema definition APIs.
- Alternative heavyweight object-relational mapping frameworks with active-record patterns (rejected)
  Rejected because: Introduces substantial runtime overhead and abstraction layers that diverge from idiomatic SQL execution patterns.
  When valid: Valid in enterprise systems requiring abstract domain mapping disconnected from underlying SQL dialect realities.

## Risks

- Potential drift between declarative schema index definitions and live database index state.
  Mitigation: Enforce automated migration checks and schema verification runs during continuous integration workflows.
  Owner: Persistence Engineering Team
- Tight coupling to SQLite dialect conventions if migrating to a distributed relational store in the future.
  Mitigation: Encapsulate schema definitions within dedicated persistence packages to isolate dialect-specific imports from upstream domain services.
  Owner: Architecture Review Board

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Isolate schema definitions within dedicated persistence packages to prevent database driver leaks into upstream domain components.
- Colocate table column definitions with composite index declarations to ensure relational structures remain unified and maintainable.

## Continuation Context


Verify commands:
- Discover the project typecheck verification task from the manifest and execute it to confirm schema definitions compile without type errors.
- Discover the database migration or schema check command configured in the project scripts and execute it to validate table definitions and index expressions against database dialect rules.

Accept when:
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

## Enforcement

- Verified by: Static analysis and typecheck verification within continuous integration pipelines.
- Verified by: Peer code review on all pull requests introducing or modifying persistence schema definitions.
- Violation handling: Continuous integration job failures for any type inconsistencies or schema verification errors.
- Violation handling: Mandatory remediation of non-compliant schema declarations prior to merge approval.
- Exception process: Submit an architectural RFC outlining the functional limitation of the adopted ORM schema builder.
- Exception process: Obtain formal review and sign-off from the persistence architecture lead before introducing custom persistence bypasses.