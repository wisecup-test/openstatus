---
version: 2
name: 7efe2696-de50-4400-979c-6a67df5fca22-adoption-of-openstatus-db-for-database-schema-definitions-database-write-operations-within-tracking-routines
description: "Adoption of @openstatus/db for Database Schema Definitions: Database Write Operations Within Tracking Routines"
metadata:
  actual:
    adr_id: 7efe2696-de50-4400-979c-6a67df5fca22
    status: accepted
    candidate_group_id: 03712c0e-1f41-4ab3-8e08-9e8c10499321
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of @openstatus/db for Database Schema Definitions: Database Write Operations Within Tracking Routines

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tracker`

Applies to:
- `src/**`

## Context

- Telemetry and event tracking systems require stable, strongly typed representations of persisted data models to record incoming events consistently.
- Decentralized or duplicate schema definitions across service boundaries introduce schema drift and validation failures as database models evolve.
- The repository architecture structures database entities into a dedicated internal workspace module, @openstatus/db, which downstream service packages consume directly.
- Tracking components integrate with localized configuration, type definitions, and filtering utilities while delegating entity structure to the shared database package.

## Problem Statement

Telemetry and tracking services must record events against persistent storage without duplicating database schema definitions or allowing ingestion models to diverge from canonical database tables.

## Decision

1. SHOULD: All database write operations within tracking routines SHOULD validate event payloads against the exported @openstatus/db schema contracts at the ingestion boundary.

## Policy Block

- SHOULD All database write operations within tracking routines SHOULD validate event payloads against the exported @openstatus/db schema contracts at the ingestion boundary.

In scope:
- Modules handling event tracking, telemetry ingestion, and persistent record creation.
- Internal packages interacting directly with database persistence schemas.

Out of scope:
- Client-facing display components and purely transient in-memory calculations with no persistence footprint.
- Third-party webhook integrations that communicate exclusively through external protocol contracts.

## Rationale

- Directly binding tracking routines to @openstatus/db schema definitions ensures persistent telemetry records remain strictly synchronized with database migrations.
- Centralizing database entity structures in a dedicated module eliminates redundant type definitions and prevents runtime insertion errors caused by model drift.
- Separating tracking configuration and filtering logic from database entities preserves modular cohesion while retaining shared schema contracts.

## Consequences

Positive:
- Guarantees type safety and structural alignment between tracking event ingestion and persistent database tables.
- Eliminates duplicate schema maintenance across distinct workspace packages.
- Simplifies database refactoring by providing a single source of truth for entity definitions.

Negative:
- Creates compile-time and build-order coupling between tracking packages and the database schema workspace module.
- Schema changes in the database package require immediate downstream updates across all consuming tracking modules.

## Alternatives

- Independent schema definitions maintained locally within the tracking package (rejected)
  Rejected because: Maintaining separate local models causes schema drift and runtime deserialization failures as the centralized database evolves.
  When valid: Only valid when isolating an external service that communicates via an immutable API contract rather than shared workspace code.
- Consuming database models exclusively via generated client code (rejected)
  Rejected because: Direct schema exports from the internal database package provide immediate compiler feedback without an additional code-generation pipeline step.
  When valid: Valid when consuming database schemas across heterogeneous programming languages or network boundaries.

## Risks

- Breaking schema changes in the database module can block build pipelines across dependent tracking packages.
  Mitigation: Enforce workspace-wide automated regression testing and coordinated versioning across package boundaries.
  Owner: engineering team
- Leaking database-specific types into public tracking interfaces could expose internal persistence details.
  Mitigation: Encapsulate schema types internally within tracking service boundaries and export sanitized domain contracts.
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
- Import database schema entities into tracking routines directly from the shared workspace package exports.
- Maintain local tracking configuration, blacklists, and utility functions in dedicated modular files within the tracking package.
- Ensure build pipelines build and resolve the database package prior to compiling downstream tracking dependencies.

## Continuation Context


Verify commands:
- # Discover the workspace package runner and execute type verification across tracking modules
- # Discover and run automated unit and integration test suites for tracking services

Accept when:
- Type verification confirms that tracking routines adhere to all imported database schema contracts without compiler errors.
- All automated test suites for tracking and telemetry operations complete successfully.
- No redundant database schema definitions are present within downstream tracking modules.

## Enforcement

- Verified by: Continuous integration static analysis and typecheck steps across workspace boundaries.
- Verified by: Peer review for all changes touching tracking data persistence or database schema dependencies.
- Violation handling: Build pipelines fail on type mismatches between tracking routines and database schema contracts.
- Violation handling: Pull requests introducing duplicate local schema definitions are rejected during code review.
- Exception process: Exceptions require architectural review and explicit sign-off from the core engineering team, accompanied by documented mitigation for schema drift.