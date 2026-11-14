---
version: 2
name: 4a25864b-6bcb-4124-8544-a72592a5a01a-adoption-of-openstatus-db-src-schema-in-notification-messaging-utilities-modules-constructing-notification-messages-source-entity
description: "Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Modules Constructing Notification Messages Source Entity"
metadata:
  actual:
    adr_id: 4a25864b-6bcb-4124-8544-a72592a5a01a
    status: accepted
    candidate_group_id: dd0719a3-2e5f-40d7-89de-6853e51b15fd
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Modules Constructing Notification Messages Source Entity

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/base`

Applies to:
- `src/utils/**`

## Context

- Notification message utilities require access to domain data model definitions for formatting messages
- The codebase maintains shared data models within the internal module @openstatus/db/src/schema
- Direct module imports allow downstream packages to share schema definitions without duplicate declarations

## Problem Statement

Notification formatting utilities need consistent, typed access to underlying database models without maintaining fragile, out-of-sync local type copies across package boundaries.

## Decision

1. MUST: Modules constructing notification messages MUST source entity data models directly from @openstatus/db/src/schema rather than re-declaring redundant local entity structures.

## Policy Block

- MUST Modules constructing notification messages MUST source entity data models directly from @openstatus/db/src/schema rather than re-declaring redundant local entity structures.

In scope:
- Notification formatting utilities requiring database entity structure representations
- Workspace modules constructing message templates from shared schema models

Out of scope:
- Standalone notification transport layers that do not interact with core schema models
- Client-facing display packages with dedicated presentation models

## Rationale

- Direct import of @openstatus/db/src/schema provides a single source of truth for domain entity structures
- Shared workspace schema imports reduce boilerplate and eliminate manual synchronization overhead across internal packages

## Consequences

Positive:
- Eliminates duplicate type declarations across package boundaries
- Ensures notification payload formatting remains synchronized with database schema definitions

Negative:
- Couples notification message formatting logic directly to internal database entity schemas
- Database schema modifications risk causing breaking changes in downstream notification utilities

## Alternatives

- Local type redeclaration in notification modules (rejected)
  Rejected because: Duplicating schema types creates drift between database entities and notification representations
  When valid: When notification modules must remain strictly decoupled from database dependencies
- Dedicated intermediate data transfer object package (deferred)
  Rejected because: Current evidence reflects direct schema consumption across package boundaries without an intermediate abstraction layer
  When valid: When cross-package schema coupling creates breaking change cascades across multiple consumers

## Risks

- Breaking changes in database schema definitions propagate immediately into notification utilities
  Mitigation: Enforce automated interface checking and contract validation across workspace boundaries
  Owner: engineering team
- Single-file observation limits architectural certainty across the wider codebase
  Mitigation: Audit neighboring notification packages to evaluate consistency of cross-package schema imports
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
- Consumers should inspect workspace dependency manifests to ensure shared schema packages are declared as workspace dependencies rather than external registry dependencies.
- Maintain schema consumption at the interface level to prevent notification templates from binding to transient database column conventions.

## Continuation Context


Verify commands:
- discover and execute the workspace type-checker to verify cross-module schema import compatibility
- discover and run the repository dependency boundary analyzer to confirm valid workspace package references

Accept when:
- Type validation succeeds with all imported schema structures resolving against shared definitions without compilation errors
- Workspace boundary checks confirm that module imports conform to package dependency rules

## Enforcement

- Verified by: Automated workspace build and static analysis checks during integration review
- Verified by: Peer architectural review of cross-package dependency additions
- Violation handling: Build failure when unresolved schema references or unauthorized cross-package imports are detected
- Violation handling: Rejection of pull requests introducing duplicated entity type declarations where shared schemas exist
- Exception process: Formal review with architecture lead documenting why local type isolation is required over shared schema imports