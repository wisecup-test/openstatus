---
version: 2
name: 88316d46-5f3c-4d19-a174-9ed894fa93ab-direct-http-response-body-handling-via-http-get-http-response-body-handling-decouple-stream
description: "Direct HTTP Response Body Handling via http.Get: Http Response Body Handling Decouple Stream"
metadata:
  actual:
    adr_id: 88316d46-5f3c-4d19-a174-9ed894fa93ab
    status: accepted
    candidate_group_id: e29e2720-9496-4aca-a344-b4e83bda4581
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Direct HTTP Response Body Handling via http.Get: Http Response Body Handling Decouple Stream

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/ssh-server`

Applies to:
- `**` (whole root)

## Context

- The service implementation directly invokes http.Get to retrieve remote payloads and inspects the resulting response body stream.
- Standard utility packages for embedding, JSON decoding, input-output streaming, formatting, and logging support the direct HTTP interaction.
- External boundary interactions and integration handling are co-located within the service entrypoint without an abstracted client or dedicated test harness.

## Problem Statement

Direct invocation of http.Get with inline consumption of response body streams tightly couples service runtime logic and integration verification to live network endpoints, increasing maintenance burden and limiting test isolation.

## Decision

1. SHOULD: HTTP response body handling SHOULD decouple stream decoding and payload deserialization from the underlying transport connection.

## Policy Block

- SHOULD HTTP response body handling SHOULD decouple stream decoding and payload deserialization from the underlying transport connection.

In scope:
- Service boundary routines that execute outgoing HTTP requests using http.Get and process response body streams.

Out of scope:
- Internal in-memory operations and static data serialization that do not perform network transport.

## Rationale

- Static analysis records direct http.Get calls and response body reading within a single service entry point alongside core serialization and input-output packages.
- Co-locating network client execution directly inside application routines without client abstraction limits integration testability and introduces coupling to remote endpoints.
- Establishing an explicit interface boundary enables isolated verification while preserving straightforward standard library stream processing.

## Consequences

Positive:
- Isolating network transport interfaces allows deterministic substitution during testing without live endpoint dependencies.
- Centralized response body handling prevents connection stream leaks across disparate execution branches.
- Integration test boundaries become explicit and independent of external service availability.

Negative:
- Introducing client abstractions increases initial boilerplate for lightweight network requests.
- Developers must maintain separate transport interfaces rather than invoking direct standard library calls.

## Alternatives

- Direct unabstracted http.Get invocations across application routines (rejected)
  Rejected because: Direct unabstracted network calls prevent deterministic integration testing and tightly couple runtime code to external network availability.
  When valid: Valid only in throwaway prototypes where automated verification and connection lifecycle management are not required.
- Heavyweight third-party HTTP client framework adoption (rejected)
  Rejected because: Standard library transport capabilities suffice when wrapped in minimal local interfaces without adding third-party dependency overhead.
  When valid: Valid when complex enterprise features like automatic circuit breaking and distributed tracing require specialized framework support.

## Risks

- Response body streams left unclosed in error handling paths cause socket exhaustion.
  Mitigation: Enforce consistent resource closing semantics immediately after checking HTTP request errors.
  Owner: Engineering Team
- Direct integration tests targeting remote endpoints fail intermittently due to network flakiness.
  Mitigation: Route integration test execution through isolated boundary mocks or local test servers.
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
- Encapsulate http.Get invocations inside client structures that accept configurable transport targets and expose decoded domain models instead of raw response bodies.
- Construct integration test suites that verify response body decoding against controlled local responses rather than live remote network endpoints.

## Continuation Context


Verify commands:
- Discover the repository build configuration and execute the standard test suite to validate HTTP client integration behavior.
- Discover static analysis and linting scripts in the project manifest and execute them to verify response body stream resource closure.

Accept when:
- All integration test targets pass cleanly without requiring active external network connections.
- Static verification confirms that all response body streams are closed across all execution paths.

## Enforcement

- Verified by: Automated continuous integration test execution and peer code reviews.
- Violation handling: Code reviews block pull requests containing direct unabstracted network calls or unclosed response body streams.
- Exception process: Submit an architectural exception request detailing why direct standard library client calls without boundary abstraction are necessary for the specific component.