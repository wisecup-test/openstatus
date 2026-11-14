---
version: 2
name: 837cdf5f-da01-4c10-b0ff-de09a408e5bd-notification-payload-schema-validation-via-discorddataschema-notification-dispatch-handlers-validate-deserialized-payload
description: "Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Validate Deserialized Payload"
metadata:
  actual:
    adr_id: 837cdf5f-da01-4c10-b0ff-de09a408e5bd
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Validate Deserialized Payload

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/discord`

Applies to:
- `src/**`

## Context

- Notification events arrive with serialized payload data strings that represent external event triggers.
- Directly consuming untyped or unverified deserialized JSON structures exposes external client integrations to malformed data, schema drift, and unexpected runtime exceptions during HTTP dispatch.
- The notification handler adopts an explicit schema parsing boundary using discordDataSchema.parse to validate the payload structure before external client invocation.

## Problem Statement

Raw serialized notification data ingested from upstream message queues or persistent storage lacks compile-time type guarantees and runtime structural integrity checks. Ingesting untyped event data directly into external HTTP delivery pipelines risks transmitting malformed payloads or failing unpredictably during downstream webhook dispatch.

## Decision

1. MUST: Notification dispatch handlers MUST validate all deserialized payload data through discordDataSchema.parse prior to constructing external client request bodies or initiating external transmissions.

## Policy Block

- MUST Notification dispatch handlers MUST validate all deserialized payload data through discordDataSchema.parse prior to constructing external client request bodies or initiating external transmissions.

In scope:
- Notification handling modules processing serialized event data for external client dispatch
- Integration boundaries transforming stored event data into external service payloads

Out of scope:
- Internal data flows where payload structures are statically guaranteed by trusted domain models
- Modules that do not deserialize untrusted or external event strings

## Rationale

- Executing discordDataSchema.parse immediately after deserializing notification data establishes a defensive perimeter that protects outbound HTTP integrations from malformed event structures.
- Parsing at the boundary guarantees that subsequent payload construction logic operates on validated data structures without requiring redundant downstream defensive checks.
- Enforcing validation before network dispatch prevents partial execution states and invalid transmissions to third-party endpoints.

## Consequences

Positive:
- Prevents malformed or corrupted notification payloads from reaching external HTTP endpoints.
- Ensures type safety and structural correctness throughout downstream payload transformation and dispatch logic.
- Isolates schema validation failures to the ingestion boundary, enabling straightforward error attribution.

Negative:
- Introduces minor CPU overhead per notification event due to string deserialization and schema parsing.
- Rejects payloads immediately upon schema discrepancies, requiring upstream data migrations when payload schemas evolve.

## Alternatives

- Ad-hoc property inspection and conditional null checks on deserialized JSON objects (rejected)
  Rejected because: Manual property checks are error-prone, incomplete, and fail to provide comprehensive runtime type safety across nested payload fields
  When valid: Lightweight scripts with strictly flat payloads where schema definitions are unavailable
- TypeScript type assertions without runtime validation (rejected)
  Rejected because: Type assertions are erased at compile time and provide zero runtime protection against unexpected payload formats
  When valid: Pure internal memory calls where payload generation and consumption share identical runtime execution contexts

## Risks

- Payload structure drift between upstream event producers and discordDataSchema can lead to unexpected parse rejections and dropped notifications
  Mitigation: Synchronize schema updates across event producer and consumer boundaries and monitor schema validation rejection rates
  Owner: engineering team
- Malformed JSON string input will throw syntax errors during initial string parsing before reaching schema validation
  Mitigation: Wrap deserialization and schema parsing routines in structured boundary error handling
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
- Locate the schema definition for discordDataSchema within the repository modules and verify all required properties before invoking the parse method.
- Ensure that deserialization errors and schema parse errors are distinguished in monitoring telemetry to facilitate debugging.

## Continuation Context


Verify commands:
- Discover the repository type check script from the project manifest and execute it to verify all payload references satisfy schema parsing constraints.
- Discover the repository test script from the project manifest and execute the test suite covering notification validation routines.

Accept when:
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and validation scripts on pull requests
- Verified by: Peer code review confirming all external client dispatch handlers validate deserialized data through schema parsing
- Violation handling: Pull requests containing unvalidated payload dispatch are blocked from merging until schema parsing is introduced
- Violation handling: Any unvalidated external dispatch identified in code review must be refactored to parse payloads through the designated schema
- Exception process: Exceptions require documented justification and formal review by security and architecture leads specifying alternative boundary controls.