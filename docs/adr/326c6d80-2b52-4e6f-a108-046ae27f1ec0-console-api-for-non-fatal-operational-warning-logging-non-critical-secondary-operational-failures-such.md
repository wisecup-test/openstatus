---
version: 2
name: 326c6d80-2b52-4e6f-a108-046ae27f1ec0-console-api-for-non-fatal-operational-warning-logging-non-critical-secondary-operational-failures-such
description: "Console API for Non-Fatal Operational Warning Logging: Non Critical Secondary Operational Failures Such"
metadata:
  actual:
    adr_id: 326c6d80-2b52-4e6f-a108-046ae27f1ec0
    status: accepted
    candidate_group_id: ed557730-6a2b-4f7c-9bb3-e3efb7e036aa
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/agent-tools/**
      - src/auth/**
      - src/external-service-component/**
      - src/external-service/**
      - src/integration/**
      - src/page-subscriber/**
---

# Console API for Non-Fatal Operational Warning Logging: Non Critical Secondary Operational Failures Such

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/agent-tools/**`
- `src/auth/**`
- `src/external-service-component/**`
- `src/external-service/**`
- `src/integration/**`
- `src/page-subscriber/**`

## Context

- Service operations frequently encounter non-fatal anomalies, including malformed database records, failed schema validations on external service configurations, and failed secondary notifications.
- Terminating or rolling back primary transactions for non-critical secondary errors or corrupted individual records would degrade service availability.
- The codebase currently records operational diagnostics directly via the runtime console warning facility passing descriptive messages and contextual metadata.
- No dedicated logging library or centralized operational logging module is imported within the service modules.

## Problem Statement

Service routines require a consistent mechanism to report non-fatal operational anomalies, background notification failures, and data parsing discards without interrupting primary execution flows or coupling service packages to unverified external logging infrastructures.

## Decision

1. MUST_NOT: Non-critical secondary operational failures, such as notification delivery errors or individual record schema validation failures during collection retrieval, MUST NOT terminate the primary service execution flow.

## Policy Block

- MUST_NOT Non-critical secondary operational failures, such as notification delivery errors or individual record schema validation failures during collection retrieval, MUST NOT terminate the primary service execution flow.

In scope:
- Service layer business logic handling non-fatal failures, background notification dispatches, and data transformation discards.
- Authentication scope validation failures and authorization access denial reporting.

Out of scope:
- Fatal service errors that require throwing domain exceptions and aborting database transactions.
- Client-facing response formatting and API error serialization.

## Rationale

- Direct runtime console logging introduces zero external package dependencies and functions universally across execution runtimes.
- Capturing contextual diagnostic payloads alongside warning messages preserves operational visibility into data inconsistencies without blocking primary read and write operations.
- Isolating non-fatal notification and webhook failures behind warning logs prevents secondary integration failures from causing primary transaction rollbacks.

## Consequences

Positive:
- Primary service workflows maintain high availability despite corrupt data rows or external notification timeouts.
- Diagnostic context is immediately emitted to runtime output streams without requiring logging client initialization.
- Operational visibility into malformed database entities and authorization denials is preserved for debugging.

Negative:
- Console output lacks centralized log routing, dynamic level filtering, and standardized structured schema ingestion.
- High-frequency warning emission during database scan operations can pollute runtime process output streams.
- Absence of a shared logging abstraction increases refactoring overhead should centralized log shipping be introduced.

## Alternatives

- Adopting a centralized external structured logging library across all service packages (rejected)
  Rejected because: Introducing an external logging package dependency across shared service packages introduces additional configuration overhead without immediate necessity for current runtime execution.
  When valid: Valid when multi-destination log aggregation, distributed trace propagation, or dynamic runtime level reconfiguration is required.
- Raising terminating exceptions and rolling back transactions on all secondary operational failures (rejected)
  Rejected because: Terminating user requests due to background notification timeouts or individual corrupt collection rows severely degrades service reliability and availability.
  When valid: Valid when strict data integrity requires transactional atomicity between primary mutations and secondary external dispatches.

## Risks

- Inadvertent logging of sensitive tenant information or authentication data within contextual warning payloads.
  Mitigation: Conduct peer code reviews and enforce automated static analysis rules to sanitize logged metadata objects.
  Owner: Engineering Team
- Unbounded console warning volume during repetitive processing of malformed batch records degrading runtime performance.
  Mitigation: Implement query-level filtering and batch error aggregation to limit repeated warning emissions.
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
- Inspect service handlers to ensure non-critical notification promises and async dispatches catch errors locally and forward them to warning emission rather than leaking unhandled rejections.
- Format warning metadata payloads as structured dictionary objects with explicit entity identifiers to assist log parsing systems.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting script to validate warning log structure across service modules.
- Discover and run the project test suite to verify that non-fatal notification and schema validation failures emit warning logs without throwing unhandled exceptions.

Accept when:
- Static analysis passes with zero disallowed log levels and no unsanitized credential references in metadata payloads.
- All service unit and integration tests succeed without unhandled promise rejections during simulated secondary notification failures.

## Enforcement

- Verified by: Continuous integration static analysis checks scanning service source files for logging compliance.
- Verified by: Peer code review verification of warning payloads and error handling boundaries.
- Violation handling: Pull requests containing unhandled non-fatal rejections or raw console error dumps in place of warning logging must be blocked until remediated.
- Exception process: Exceptions requiring silent error suppression or alternative log routing require approval from the principal engineering team.