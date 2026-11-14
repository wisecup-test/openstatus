---
version: 2
name: a2e1e326-250b-4089-bcc8-549595421e1b-adoption-of-openstatus-assertions-module-for-monitor-validation-and-serialization-persistence-layers-store-monitor-assertion-collections
description: "Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Persistence Layers Store Monitor Assertion Collections"
metadata:
  actual:
    adr_id: a2e1e326-250b-4089-bcc8-549595421e1b
    status: accepted
    candidate_group_id: 18e039bc-6f1d-4be9-be61-61fcfc11a203
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/assertions Module for Monitor Validation and Serialization: Persistence Layers Store Monitor Assertion Collections

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Monitor configuration endpoints require rigorous validation and serialization of assertion rules including HTTP status, header values, and body content checks.
- Multiple server ingress paths, including REST API route handlers and RPC converter services, process monitor assertions independently.
- Direct JSON parsing without centralized domain model enforcement introduces schema divergence between API ingestion and monitor runner execution.

## Problem Statement

Monitor definitions across disparate HTTP routes and RPC services require consistent assertion validation, domain typing, and serialization, but without a dedicated domain library, ingestion endpoints risk implementing divergent schema validation and incompatible serialization structures.

## Decision

1. MUST: Persistence layers MUST store monitor assertion collections using the serialize utility from @openstatus/assertions rather than unformatted object stringification.

## Policy Block

- MUST Persistence layers MUST store monitor assertion collections using the serialize utility from @openstatus/assertions rather than unformatted object stringification.

In scope:
- All HTTP route handlers processing monitor creation and modification operations.
- RPC converters translating wire protocol payloads to internal monitor representations.
- Monitor persistence and utility functions handling assertion serialization.

Out of scope:
- Non-monitor routes and monitoring modules that do not define or process assertion evaluation rules.
- Telemetry ingestion and analytics processing that operate strictly on downstream metric outcomes.

Exceptions:
- EXC-20-001: Transitional migration of legacy monitor definitions requiring legacy raw schema fallback.

## Rationale

- Evidence across seven server files demonstrates consistent reliance on @openstatus/assertions for assertion parsing, type construction, and serialization.
- Encapsulating assertion logic within a dedicated workspace module prevents duplication and prevents subtle parsing divergences between REST and RPC boundaries.
- Standardized serialization guarantees uniform persistence structures across monitor creation and update routines.

## Consequences

Positive:
- Single source of truth for monitor assertion rules across REST API endpoints and RPC execution handlers.
- Guaranteed schema consistency and type safety across monitor creation, update, and conversion operations.
- Deterministic serialization format across all monitor configuration storage targets.

Negative:
- Tight coupling between monitor route definitions and internal assertion domain models requiring lockstep version updates.
- Additional deserialization overhead when converting database-stored assertion payloads into domain class instances during runtime execution.

## Alternatives

- Inline route-specific assertion schemas and parsing routines (rejected)
  Rejected because: Leads to schema drift and validation mismatches between HTTP ingestion endpoints and RPC monitoring execution paths.
  When valid: Only permissible in throwaway prototypes without downstream monitoring runners.
- Generic schema validator without domain-specific assertion classes (rejected)
  Rejected because: Lacks encapsulation for domain-specific evaluation logic, versioning tags, and standard serialization routines.
  When valid: Valid when assertion criteria do not require versioned serialization or typed execution runners.

## Risks

- Breaking changes to assertion schemas can invalidate previously serialized monitor configurations.
  Mitigation: Enforce schema version identifiers in assertion class constructors and maintain backward-compatible parser fallbacks.
  Owner: Engineering Team
- Parsing malformed user-supplied assertion JSON can produce unhandled exceptions during RPC conversion.
  Mitigation: Catch parsing errors explicitly and record error context in application logging before bubbling sanitized errors to API clients.
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
- Instantiate assertion instances using appropriate domain constructors before appending to monitor configuration collections.
- Ensure all database persistence operations invoke serialize from the assertions module prior to record storage.
- Capture and log raw payload fragments alongside parsing failure diagnostics to assist monitor debugging.

## Continuation Context


Verify commands:
- Discover and execute the project workspace test suite to verify assertion validation and parsing tests pass.
- Run the repository static analysis and type checker to confirm conformance with @openstatus/assertions types across all monitor route handlers and converters.

Accept when:
- All monitor route handlers and converters compile with shared assertion module imports.
- Type checking and assertion schema parsing tests pass with zero validation mismatches across test suites.
- Serialized monitor assertion payloads conform to the canonical assertions module representation.

## Enforcement

- Verified by: Continuous integration static type analysis and test suite execution.
- Verified by: Automated pull request linting and import boundary verification.
- Verified by: Peer architecture review on changes touching monitor route handlers or RPC converters.
- Violation handling: Pull requests with direct ad-hoc assertion parsing or manual serialization are blocked at code review.
- Violation handling: Continuous integration test failures reject builds containing unregistered assertion types.
- Exception process: Architecture review team evaluation and approval before any non-standard assertion format may be used.
- Exception process: Documented deprecation timeline if legacy assertion payload structures require transitional handling.