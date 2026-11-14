---
version: 2
name: cbae54fa-63a6-493d-b7be-7bcc49f37c3a-schema-based-input-validation-via-googlechatdataschema-parse-for-notification-payloads-outbound-webhook-clients-not-dispatch-http
description: "Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Outbound Webhook Clients Not Dispatch Http"
metadata:
  actual:
    adr_id: cbae54fa-63a6-493d-b7be-7bcc49f37c3a
    status: accepted
    candidate_group_id: 46c9d68e-5833-432a-8164-b85f8973d04a
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Outbound Webhook Clients Not Dispatch Http

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/google-chat`

Applies to:
- `src/**`

## Context

- Notification integration handlers receive serialized payload data strings that represent external dispatch configurations and content.
- Direct consumption of unvalidated deserialized data risks runtime type errors and malformed external webhook requests.
- Static analysis identified schema parsing on deserialized notification payloads in googleChatDataSchema.parse prior to outbound client execution.

## Problem Statement

Deserializing unvalidated notification data payloads directly into outbound HTTP requests risks dispatching malformed payloads or encountering unhandled runtime exceptions during notification processing.

## Decision

1. MUST_NOT: Outbound webhook clients MUST NOT dispatch HTTP requests using unvalidated notification payload data.

## Policy Block

- MUST_NOT Outbound webhook clients MUST NOT dispatch HTTP requests using unvalidated notification payload data.

In scope:
- Incoming notification payload handling routines in notification delivery packages.
- Deserialization of external or persisted notification data prior to outbound integration requests.

Out of scope:
- Internal service communication where payloads are already strongly typed in-memory objects.
- Outbound client payload generation where data schemas have already been validated at entry boundaries.

## Rationale

- Validating deserialized data structures through googleChatDataSchema.parse ensures all required notification properties conform to contract expectations before network calls.
- Isolating validation to integration boundary entry points prevents corrupted payload persistence and simplifies error containment.

## Consequences

Positive:
- Guarantees runtime data integrity for outbound webhook payloads.
- Catches malformed configuration or corrupted notification data before external network requests occur.
- Centralizes validation logic within declarative schema definitions.

Negative:
- Adds JSON parsing and schema evaluation overhead to notification dispatch execution.
- Requires updating schema definitions whenever integration payload structures evolve.

## Alternatives

- Manual runtime type guards and property checking (rejected)
  Rejected because: Manual property checks are verbose, error-prone, and difficult to keep consistent across multiple payload attributes.
  When valid: Small utility functions evaluating single primitive values without complex nested structures.
- Direct type assertion without runtime validation (rejected)
  Rejected because: Type assertions bypass runtime checks, permitting malformed data to cause uncaught errors during external API invocation.
  When valid: Internal trusted interfaces where upstream components have already validated payload structures.

## Risks

- Schema parsing errors causing silent notification drops if error handling does not notify monitoring systems.
  Mitigation: Log parsing failures to error telemetry and dispatch failure notifications to operational alerts.
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
- Ensure schema definitions match the payload contract expected by notification webhook consumers and wrap parsing logic in try-catch structures.

## Continuation Context


Verify commands:
- Discover the test runner from the project manifest and run automated tests targeting notification payload validation suites.
- Discover the static analysis and type verification script from the repository configuration and execute project verification.

Accept when:
- All schema validation test cases covering valid and malformed payload inputs pass without regression.
- Static type verification and lint analysis succeed across all notification packages without errors.

## Enforcement

- Verified by: Automated unit tests validating payload schema acceptance and rejection behavior.
- Verified by: Continuous integration type checking and peer code review.
- Violation handling: Pull requests lacking runtime schema validation on deserialized integration payloads will be blocked during review.
- Exception process: Exceptions require documented architecture review demonstrating upstream runtime validation guarantees.