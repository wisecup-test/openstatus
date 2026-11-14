---
version: 2
name: 7451afce-552d-4265-9d2c-6b62f9a327a4-drizzle-orm-sqlite-schema-modeling-before-integrating-schema-libraries-developers-inspect
description: "Drizzle ORM SQLite Schema Modeling: Before Integrating Schema Libraries Developers Inspect"
metadata:
  actual:
    adr_id: 7451afce-552d-4265-9d2c-6b62f9a327a4
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Before Integrating Schema Libraries Developers Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/viewers/**`

## Context

- The project requires persistent data storage for identity and session management using an embedded or relational SQLite database.
- Data access layers benefit from declarative, type-safe schemas that align TypeScript types directly with database table structures.
- Authentication and authorization subsystems require standardized user and session entities with referential integrity and timestamp handling.

## Problem Statement

Managing relational schema definitions without centralized, type-safe specifications leads to type drift between database columns and application models, increasing the risk of orphaned session records and inconsistent timestamp representations across persistence queries.

## Decision

1. MUST: Before integrating schema libraries, developers MUST inspect the repository dependency resolution lock artifact to identify the exact resolved version and verify supported API signatures.

## Policy Block

- MUST Before integrating schema libraries, developers MUST inspect the repository dependency resolution lock artifact to identify the exact resolved version and verify supported API signatures.

In scope:
- All data persistence schemas and table definitions utilizing SQLite within the repository data layer.
- Authentication, session, and user identity persistence models.

Out of scope:
- In-memory transient application state not persisted to the relational database.
- Non-relational data storage mechanisms or external third-party API response shapes.

## Rationale

- Drizzle ORM SQLite core primitives provide lightweight, zero-overhead type inference that directly translates TypeScript types to SQL schema constraints without requiring code generation steps.
- Explicit foreign key references with cascading deletes ensure that dependent session tokens are automatically cleaned up when parent identities are removed.
- Specifying timestamp modes at the column definition level eliminates manual conversion between epoch integers and Date objects across application queries.

## Consequences

Positive:
- Compile-time validation ensures that database queries and mutations remain synchronized with underlying SQLite table definitions.
- Automated cascading deletions prevent orphan session records in identity storage.
- Consistent timestamp handling reduces time-zone conversion bugs and serialization inconsistencies.

Negative:
- Schema models are coupled to SQLite dialect primitives, requiring refactoring if migrating to alternative SQL dialects.
- Developers must learn library-specific column declaration and query syntax.

## Alternatives

- Raw SQL DDL migrations with manually authored TypeScript interfaces (rejected)
  Rejected because: Manual synchronization between raw SQL schema statements and TypeScript interfaces is error-prone and leads to type drift over time.
  When valid: Valid when operating in environments where ORM libraries cannot be executed or external migration tooling is mandated.
- Heavy runtime Object-Relational Mapping frameworks (rejected)
  Rejected because: Runtime metadata reflection and active-record overhead introduce unnecessary bundle size and execution complexity for embedded SQLite operations.
  When valid: Valid in complex multi-database enterprise applications requiring distributed transactions and complex identity map patterns.

## Risks

- Dialect lock-in to SQLite specific column builders and SQL expressions limits database portability.
  Mitigation: Isolate dialect-specific definitions within dedicated schema modules behind standard repository access interfaces.
  Owner: engineering team
- Mismatched timestamp serialization modes could cause runtime date parsing failures.
  Mitigation: Enforce uniform timestamp modes and validate date serialization through automated integration tests.
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
- Group related domain tables and their foreign key relations into dedicated schema files within the database module.
- Use SQL template helpers for database-evaluated defaults such as current timestamp generation during record insertion.

## Continuation Context


Verify commands:
- Discover and run the project type checking script to verify schema type correctness across all consuming packages.
- Discover and run the database schema verification or migration generation command to validate table constraints.
- Discover and run the automated test suite to confirm session cascading and timestamp deserialization.

Accept when:
- Static type analysis passes with zero errors across all database schema definitions and downstream consumers.
- Database schema migration checks confirm valid SQLite DDL generation with foreign key references intact.
- Integration tests succeed confirming cascade deletion of session records upon user deletion.

## Enforcement

- Verified by: Automated continuous integration checks executing static type validation and schema verification scripts.
- Verified by: Peer code reviews confirming compliance with schema modeling conventions and referential constraint rules.
- Violation handling: Pull requests with failing type checks, missing foreign key cascades, or invalid column definitions are blocked from merging.
- Violation handling: Violations identified during code review require remediation before approval.
- Exception process: Exceptions for non-standard schema constructs must be documented in an architectural exception ticket and approved by database leads.