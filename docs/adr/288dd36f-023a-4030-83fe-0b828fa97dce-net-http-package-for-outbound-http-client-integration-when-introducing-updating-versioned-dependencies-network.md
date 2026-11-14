---
version: 2
name: 288dd36f-023a-4030-83fe-0b828fa97dce-net-http-package-for-outbound-http-client-integration-when-introducing-updating-versioned-dependencies-network
description: "net/http Package for Outbound HTTP Client Integration: When Introducing Updating Versioned Dependencies Network"
metadata:
  actual:
    adr_id: 288dd36f-023a-4030-83fe-0b828fa97dce
    status: accepted
    candidate_group_id: b2869a74-327c-41e1-86c9-428c30e1d0a1
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/tinybird/**
      - internal/workflows/**
---

# net/http Package for Outbound HTTP Client Integration: When Introducing Updating Versioned Dependencies Network

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/tinybird/**`
- `internal/workflows/**`

## Context

- Outbound service integrations require consistent transport configuration, credential handling, and context propagation.
- Internal client packages encapsulate external API communication boundaries using core net/http primitives.
- Payload marshaling and header injection follow uniform structural patterns across distinct service integration components.

## Problem Statement

Direct or disparate implementations of outbound HTTP requests across internal service packages risk inconsistent credential handling, unmanaged connection lifecycles, and uncoordinated context cancellation.

## Decision

1. MUST: When introducing or updating versioned dependencies for network integrations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version prior to implementation.

## Policy Block

- MUST When introducing or updating versioned dependencies for network integrations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version prior to implementation.

In scope:
- Outbound HTTP service integrations and API client adapters residing within internal application packages.

Out of scope:
- Inbound HTTP server handlers, in-memory domain service boundaries, and non-HTTP protocol communication clients.

## Rationale

- Standard library net/http primitives provide complete HTTP client functionality without external dependency risks.
- Encapsulating HTTP requests inside dedicated client structures guarantees uniform header injection, payload encoding, and error response inspection.
- Context propagation through standard client methods ensures cancellation signals and execution timeouts flow reliably across service boundaries.

## Consequences

Positive:
- Eliminates third-party client dependencies for core external service communications.
- Standardizes request dispatch, context propagation, and authentication header management across integration clients.
- Provides uniform testability through standard HTTP client encapsulation interfaces.

Negative:
- Boilerplate request construction, header assignment, and payload serialization must be maintained explicitly across client structs.
- Advanced capabilities such as automated retries, circuit breaking, and backoff policies require custom implementation rather than configuration.

## Alternatives

- Third-party HTTP client libraries (rejected)
  Rejected because: Introduces external dependency maintenance burden and expands vulnerability surface when standard library capabilities fulfill integration requirements.
  When valid: Advanced transport requirements such as automated distributed tracing interceptors or custom connection pooling fail to be supported by standard library interfaces.
- Inline HTTP calls embedded directly within business domain services (rejected)
  Rejected because: Couples domain business logic directly to transport mechanics, header manipulation, and payload serialization protocols.
  When valid: Single-execution throwaway scripts where isolation boundaries provide no maintenance value.

## Risks

- Omission of response body closure leading to socket descriptor leaks and connection pool starvation.
  Mitigation: Mandate deferral or immediate invocation of response body closure following successful request execution checks.
  Owner: engineering team
- Unbounded network request duration causing resource exhaustion when remote services stall.
  Mitigation: Enforce context deadline propagation across all client invocation signatures.
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
- Encapsulate net/http Client references within internal client structures initialized through dedicated constructor functions.
- Always inject authorization credentials into request headers during request construction rather than exposing them in query parameters.
- Ensure response bodies are consistently closed after inspecting execution errors.

## Continuation Context


Verify commands:
- Discover the test execution entry point from repository configuration and run test suites covering outbound client packages.
- Discover the static analysis tool configured in the repository and verify compliance of all HTTP client implementations.

Accept when:
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

## Enforcement

- Verified by: Automated static analysis and code review checks verifying that external calls route through encapsulated client structs.
- Verified by: Repository test suites validating context deadline handling and response body closure.
- Violation handling: Pull requests introducing unencapsulated HTTP calls or unmanaged client instances are blocked until refactored.
- Violation handling: Code review flags instances where response bodies are left unclosed or context parameters are omitted.
- Exception process: Submit an architectural review request detailing the functional limitations of the standard client wrapper.
- Exception process: Document explicit security and dependency justifications if an alternative communication library is proposed.