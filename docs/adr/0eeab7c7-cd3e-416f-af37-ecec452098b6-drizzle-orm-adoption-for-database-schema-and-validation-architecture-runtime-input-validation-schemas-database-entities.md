---
version: 2
name: 0eeab7c7-cd3e-416f-af37-ecec452098b6-drizzle-orm-adoption-for-database-schema-and-validation-architecture-runtime-input-validation-schemas-database-entities
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Runtime Input Validation Schemas Database Entities"
metadata:
  actual:
    adr_id: 0eeab7c7-cd3e-416f-af37-ecec452098b6
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Runtime Input Validation Schemas Database Entities

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/external_services/**`

## Context

- The persistent data layer requires cohesive relational table declarations with explicit compound index definitions across relational foreign identifiers and creation timestamps.
- Domain entities require runtime schema validation to ensure incoming payload integrity matches underlying database column constraints.
- Separate authoring of relational database structures and validation schemas introduces drift and maintenance overhead between persistence schemas and runtime validation boundaries.
- The codebase adopts drizzle-orm alongside drizzle-orm/sqlite-core and derives runtime validation schemas using drizzle-zod and zod.

## Problem Statement

Defining relational database tables, compound indexes, and input validation models independently leads to schema synchronization drift, redundant type declarations, and potential runtime constraint violations across database modules.

## Decision

1. MUST: Runtime input validation schemas for database entities MUST be derived directly from table schemas using drizzle-zod and zod to prevent contract divergence between persistence and validation boundaries.

## Policy Block

- MUST Runtime input validation schemas for database entities MUST be derived directly from table schemas using drizzle-zod and zod to prevent contract divergence between persistence and validation boundaries.

In scope:
- Relational table schema definitions, database indexing, and domain model validation across persistence packages.

Out of scope:
- Pure business domain logic and external communication contracts operating outside persistent data storage and schema validation boundaries.

## Rationale

- Utilizing drizzle-orm with composite indexing declarations establishes declarative, type-safe database schemas with optimized query access paths.
- Integrating drizzle-zod with zod enables automated generation of runtime validation schemas directly from table definitions, eliminating schema duplication and drift.
- Static code evidence demonstrates co-located schema definitions and validation layers across domain entity modules within the database package.

## Consequences

Positive:
- Single source of truth for persistent entity structures and runtime validation contracts.
- High type safety across persistence definitions, compound index declarations, and domain input validation.
- Reduced maintenance overhead by eliminating manual duplication between database schemas and validation models.

Negative:
- Direct dependency coupling across the schema layer to drizzle-orm, drizzle-zod, and zod.
- Schema evolution requires coordinating table modifications with downstream derived validation schema expectations.

## Alternatives

- Raw SQL DDL definitions and manual interface declarations (rejected)
  Rejected because: Lacks compile-time type safety, automated validation derivation, and increases risk of synchronization drift between persistence and runtime models.
  When valid: When operating in environments that prohibit object-relational mapping libraries or require raw database engine introspection.
- Decoupled manual validation schema authoring without derivation from table schemas (rejected)
  Rejected because: Forces duplicate maintenance of field definitions and types across database tables and validation schemas, increasing the likelihood of unaligned constraints.
  When valid: When external input formats intentionally diverge substantially from underlying storage representations.

## Risks

- Upstream API variations across resolved library releases could break derived schema contracts.
  Mitigation: Strictly adhere to the lock-version grounding process to verify API compatibility against resolved versions before implementing schema modifications.
  Owner: Database Engineering Team
- Improper composite index ordering in schema definitions could lead to suboptimal database query performance.
  Mitigation: Review index column sequences in accordance with query access patterns during code review.
  Owner: Database Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure composite index declarations reflect filtering and sorting access paths across relational foreign keys and timestamp attributes.
- Export derived validation schemas from dedicated schema validation modules to provide clean ingestion boundaries for consuming applications.

## Continuation Context


Verify commands:
- # Discover and execute the project database test suite to verify schema definitions and indexing
- # Discover and execute the repository type validation and linter checks to ensure schema and validation contract compliance

Accept when:
- Database table schemas compile without type errors and composite indexes match declared access patterns.
- Derived validation schemas correctly validate compliant payloads and reject malformed input matching table constraints.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository test and validation scripts.
- Verified by: Peer code review verifying that schema modifications include composite indexes and derived validation models.
- Violation handling: Pull requests containing manually duplicated validation schemas or unindexed relational query paths are rejected until aligned with Drizzle ORM and Drizzle-Zod derivation patterns.
- Exception process: Formal architecture review and team consensus required before introducing alternative persistence or validation patterns.