---
version: 2
name: 3ccd423c-ae9c-43fa-aa2c-d34858822857-zod-schema-validation-for-subscription-channel-configurations-and-payloads-notification-payload-schemas-compose-shared-domain
description: "Zod Schema Validation for Subscription Channel Configurations and Payloads: Notification Payload Schemas Compose Shared Domain"
metadata:
  actual:
    adr_id: 3ccd423c-ae9c-43fa-aa2c-d34858822857
    status: accepted
    candidate_group_id: 500f8313-2cab-4db3-a165-4a1aab0ad2dd
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Zod Schema Validation for Subscription Channel Configurations and Payloads: Notification Payload Schemas Compose Shared Domain

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/**`

## Context

- Notification delivery mechanisms process diverse subscriber configuration data retrieved from persistent stores and dispatch structured events to external targets.
- External channel configurations and notification payload payloads represent untrusted or semi-trusted data boundaries that can cause delivery failures if malformed.
- Adoption of structured schema parsing provides runtime guarantees against malformed fields, unexpected formats, and unvalidated payloads before external dispatch.

## Problem Statement

Subscription channel dispatchers receive serialized configuration strings from persistent storage and format dynamic event payloads for third-party consumers; without standardized, runtime schema validation at these input/output boundaries, invalid configurations or malformed payloads can lead to unhandled runtime crashes, silent delivery drops, or security vulnerabilities.

## Decision

1. SHOULD: Notification payload schemas SHOULD compose shared domain validation schemas to prevent divergent schema definitions across distinct channel implementations.

## Policy Block

- SHOULD Notification payload schemas SHOULD compose shared domain validation schemas to prevent divergent schema definitions across distinct channel implementations.

In scope:
- All subscription notification channel adapters handling deserialized configuration data.
- All status report and event notification payload definitions dispatched to external targets.

Out of scope:
- Internal utility functions operating exclusively on statically verified internal memory structures.
- Direct database schema migrations managed by dedicated migration frameworks.

Exceptions:
- EXC-42-001: A channel requires dynamic passthrough headers that cannot be strictly enumerated in advance.

## Rationale

- Declarative schema parsing using Zod allows boundary contracts to be verified at runtime while keeping type definitions in sync with application logic.
- Using safe parsing methods ensures that malformed channel configurations fail predictably without throwing unhandled exceptions that disrupt background dispatch workers.
- Centralized payload schemas ensure that status report structures remain consistent across email, webhook, and chat notification channels.

## Consequences

Positive:
- Guarantees structural validity and type safety of untrusted channel configurations and outgoing payloads at runtime boundaries.
- Prevents malformed data and unexpected null values from triggering unhandled failures during delivery dispatch.
- Centralizes contract definitions for notification payloads, easing schema evolution and versioning across channel adapters.

Negative:
- Introduces a modest runtime evaluation overhead for schema parsing during notification processing.
- Requires developers to maintain synchronized schema contracts as notification payloads and channel settings evolve.

## Alternatives

- Manual runtime assertion checks with bespoke conditional logic (rejected)
  Rejected because: Manual defensive assertions are error-prone, drift over time, and do not provide cohesive schema-driven type guarantees.
  When valid: Valid only in minimal modules where introducing an external dependency is prohibited.
- Permissive parsing without boundary validation relying solely on static compile-time typing (rejected)
  Rejected because: Compile-time typing is erased at runtime and cannot protect against malformed dynamic inputs from external endpoints or persistent storage.
  When valid: Valid only for purely internal, statically linked in-memory transformations where external boundaries are absent.

## Risks

- Overly strict validation schemas may reject legacy configuration formats stored in the database.
  Mitigation: Enforce strict unit tests for validation schemas and maintain versioned schemas for backward compatibility.
  Owner: engineering team
- Performance degradation in high-volume notification loops due to deep object parsing.
  Mitigation: Benchmark schema parsing in high-frequency paths and keep schemas focused on boundary verification.
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
- Define channel configuration schemas alongside their respective channel handlers to maintain high cohesion.
- Use non-throwing safe parsing methods when evaluating configuration data to handle malformed input gracefully.
- Compose complex event payload schemas from reusable domain primitive schemas rather than defining inline duplicates.

## Continuation Context


Verify commands:
- test -n "${VERIFY_SCHEMA_VALIDATION:-}" && eval "${VERIFY_SCHEMA_VALIDATION}" || echo "Discover and execute project test suite for schema validation compliance"
- test -n "${VERIFY_CODE_STANDARDS:-}" && eval "${VERIFY_CODE_STANDARDS}" || echo "Discover and execute project static analysis and linting checks"

Accept when:
- All channel configuration parsing and payload formatting paths pass automated boundary validation tests with valid schemas.
- Invalid configuration strings or corrupted event payloads fail safely without uncaught runtime exceptions during dispatch.
- The locked dependency version for the schema validation library is verified against repository lock artifacts prior to execution.

## Enforcement

- Verified by: Automated unit and integration test suites validating schema parsing across all channel adapters.
- Verified by: Static analysis and code review checks confirming that all incoming external data passes through validation routines.
- Violation handling: Merge requests that deserialize external channel configurations without schema validation MUST be blocked.
- Violation handling: Runtime validation failures must be logged at the error level and fail safely without taking down dispatch workers.
- Exception process: Submit an architectural review request detailing the boundary constraints that prevent schema-driven parsing.
- Exception process: Obtain approval from the security and architecture review owners prior to merging unvalidated boundaries.
- Exception process: Document the compensating runtime validation controls in the module's documentation.