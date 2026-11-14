---
version: 2
name: 443f45cd-04aa-40f8-97a3-dcc15cd2bb9f-adoption-of-openstatus-db-src-schema-internal-database-schema-module-consuming-modules-isolate-schema-dependent-conversions
description: "Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Modules Isolate Schema Dependent Conversions"
metadata:
  actual:
    adr_id: 443f45cd-04aa-40f8-97a3-dcc15cd2bb9f
    status: accepted
    candidate_group_id: 20d7a85b-14ec-4581-82b0-2748cc7d83d2
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Modules Isolate Schema Dependent Conversions

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/base`

Applies to:
- `src/utils/**`

## Context

- Notification domain logic requires structured access to incident entities backed by the persistence tier.
- The internal workspace module @openstatus/db/src/schema maintains centralized database schema definitions for the system.
- Static analysis detected consumption of @openstatus/db/src/schema within notification utility modules.

## Problem Statement

Notification utilities require access to incident data definitions, creating a risk of contract divergence if packages maintain independent models instead of referencing a shared database schema.

## Decision

1. SHOULD: Consuming modules SHOULD isolate schema-dependent conversions within dedicated boundary utilities to minimize ripple effects from schema migrations.

## Policy Block

- SHOULD Consuming modules SHOULD isolate schema-dependent conversions within dedicated boundary utilities to minimize ripple effects from schema migrations.

In scope:
- Internal workspace packages and utility modules that operate on database-backed domain entities.

Out of scope:
- Modules communicating across external network boundaries requiring isolated public contract definitions.

## Rationale

- Referencing @openstatus/db/src/schema ensures notification processing operates against authoritative database representations.
- Consolidating schema definitions inside the dedicated database module avoids maintenance friction across multiple workspace packages.

## Consequences

Positive:
- Establishes a single source of truth for database schema types across workspace packages.
- Eliminates redundant data model definitions and prevents synchronization drift during schema updates.

Negative:
- Consuming modules are directly exposed to internal database schema restructuring and migration changes.
- Increases cross-package build coupling across internal workspace boundaries.

## Alternatives

- Duplicate incident data structure definitions within local utility modules (rejected)
  Rejected because: Creates data model drift between notification payloads and database persistence contracts
  When valid: When notification modules must remain strictly decoupled from persistence schemas
- Export abstract data transfer objects through an intermediate shared interface package (rejected)
  Rejected because: Adds indirection and maintenance overhead when the workspace already consolidates schemas in a dedicated package
  When valid: When external consumers outside the workspace boundary require isolated API contracts

## Risks

- Changes to underlying database schemas in @openstatus/db/src/schema may inadvertently break notification utilities at compile time.
  Mitigation: Enforce automated workspace type checking to catch breaking schema adjustments before deployment.
  Owner: engineering team
- Tight coupling between persistence definitions and domain utilities could hinder independent package versioning.
  Mitigation: Define stable exported type surfaces within the schema package to shield consumers from internal table refactoring.
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
- Verify that workspace dependency manifests declare proper internal package references for @openstatus/db before importing schema members.
- Consume shared schema types directly rather than asserting structural type equivalence in local utility functions.

## Continuation Context


Verify commands:
- Discover the repository type check script declared in the project configuration manifest and execute it across all packages.
- Discover and execute the repository static analysis and test validation suites to verify cross-package schema integration.

Accept when:
- Static analysis and type checking pass across workspace packages consuming @openstatus/db/src/schema without unresolved module errors.
- Domain utility logic compiles against the shared database schema definitions with no conflicting local data contracts.

## Enforcement

- Verified by: Automated static analysis and type checking during continuous integration workflows.
- Verified by: Peer code review verification of cross-package import boundaries.
- Violation handling: Pull requests introducing duplicated data models or unapproved schema imports are blocked by automated build checks.
- Violation handling: Violations identified in review must be refactored to consume the centralized schema package.
- Exception process: Submit an architectural exception request detailing why a consuming package cannot reference the shared schema module directly.
- Exception process: Obtain approval from the lead architect prior to introducing independent persistence models.