---
version: 2
name: aaacf77d-dc23-4ac8-b70d-a2dc2a1ce80d-adoption-of-openstatus-notification-base-for-notification-dispatch-handlers-incoming-notification-payload-strings-parsed-validated
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Incoming Notification Payload Strings Parsed Validated"
metadata:
  actual:
    adr_id: aaacf77d-dc23-4ac8-b70d-a2dc2a1ce80d
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Incoming Notification Payload Strings Parsed Validated

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/google-chat`

Applies to:
- `src/**`

## Context

- Notification delivery requires translating internal event payloads into formats compatible with external recipient webhook endpoints.
- The notification subsystem incorporates shared internal modules such as @openstatus/notification-base and @openstatus/db/src/schema to structure notification handlers.
- External client boundaries require explicit payload formatting and HTTP transmission to ensure delivery across third-party communication channels.

## Problem Statement

Dispatching notifications to external platforms without explicit input validation risks sending malformed payloads, resulting in delivery failures and opaque error states at runtime boundary calls.

## Decision

1. MUST: Incoming notification payload strings MUST be parsed and validated using googleChatDataSchema.parse prior to initiating external client dispatch operations.

## Policy Block

- MUST Incoming notification payload strings MUST be parsed and validated using googleChatDataSchema.parse prior to initiating external client dispatch operations.

In scope:
- Outbound notification handler modules that adapt internal notification payloads into external client requests.

Out of scope:
- Internal service-to-service communication mechanisms that do not communicate across external network client boundaries.

## Rationale

- Parsing notification payloads through googleChatDataSchema verifies data integrity prior to external network dispatch, avoiding rejected requests.
- Isolating external client network invocations within dedicated adapter modules preserves a clear separation between internal notification models and third-party APIs.

## Consequences

Positive:
- Prevents malformed data from triggering failed outbound HTTP requests through early schema validation.
- Maintains clear structural boundaries between internal notification contracts and external provider webhook payloads.

Negative:
- Couples notification dispatchers to specific schema definitions, requiring schema synchronization when external payload requirements evolve.
- Introduces runtime validation overhead for every dispatched notification message.

## Alternatives

- Direct forwarding of raw JSON payloads without intermediate schema validation (rejected)
  Rejected because: Transmitting unvalidated payloads allows malformed data structures to reach external endpoints, complicating error diagnosis and webhook reliability.
  When valid: Only acceptable in throwaway test environments where payload validation is irrelevant.

## Risks

- External destination endpoints may be unavailable or change their acceptance contracts, causing outbound fetch calls to fail.
  Mitigation: Encapsulate fetch invocations in comprehensive error handling that logs failure diagnostics without halting unrelated worker processes.
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
- Locate the schema definitions within the repository modules to verify that outbound fields align with the provider specification.
- Verify that external network client calls incorporate proper error-catching structures to capture HTTP failures.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and type-checking scripts from the build configuration to verify schema and dispatch types.
- Discover and execute the project's unit and integration test suite to validate notification parsing and webhook dispatch logic.

Accept when:
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

## Enforcement

- Verified by: Automated build pipeline checks executing static validation and unit test suites.
- Verified by: Peer code review verification of external boundary client implementations.
- Violation handling: Pull requests that bypass schema validation or omit structured error logging will be rejected during review.
- Exception process: Exceptions require documented justification and sign-off from the technical architecture lead.