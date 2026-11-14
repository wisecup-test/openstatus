---
version: 2
name: 4ce0c76c-9a1b-4d3e-b17d-14c4a179d975-adoption-of-connect-rpc-for-service-api-implementation-before-implementing-modifying-service-handlers-that
description: "Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That"
metadata:
  actual:
    adr_id: 4ce0c76c-9a1b-4d3e-b17d-14c4a179d975
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`

## Context

- The private location server subsystem coordinates monitoring jobs, receives active probe results, and reports ingestion metrics across HTTP, TCP, and DNS protocols.
- Handler implementations across multiple endpoints require standardized authentication, structured telemetry enrichment, and consistent error code propagation.
- The codebase utilizes the Connect RPC library to expose typed service interfaces and enforce reliable boundary communication between private location probes and backend handlers.

## Problem Statement

Managing distributed probe interactions and metric ingestion requires consistent request parsing, uniform authentication extraction, type-safe payload contracts, and standardized error signaling across multiple probe protocols without incurring the complexity of heavyweight transport runtimes.

## Decision

1. MUST: Before implementing or modifying service handlers that consume the Connect RPC library, engineers MUST inspect the repository dependency manifest and lock artifact to resolve the exact dependency version and verify API compatibility against authoritative documentation.

## Policy Block

- MUST Before implementing or modifying service handlers that consume the Connect RPC library, engineers MUST inspect the repository dependency manifest and lock artifact to resolve the exact dependency version and verify API compatibility against authoritative documentation.

In scope:
- Server-side RPC handler functions processing monitoring queries and telemetry ingestion across service boundaries.
- Internal communication protocols between private location runners and core ingestion infrastructure.

Out of scope:
- Internal database storage abstraction layers and direct SQL operations.
- External third-party HTTP client adapters operating outside RPC listener endpoints.

## Rationale

- Connect RPC delivers schema-backed, type-safe RPC communication while operating over standard HTTP protocols with native generic request and response types.
- Standardized error codes provide unambiguous status signaling for authentication failures, resource misses, and internal server faults across all probe ingestion points.
- Decoupling transport protocols from strict gRPC runtime dependencies simplifies deployment across diverse private runner network topologies.

## Consequences

Positive:
- Uniform type safety and generic envelope handling across all monitoring and ingestion endpoints.
- Consistent RPC status code mapping and error delivery across service boundaries.
- Interoperability with standard HTTP and gRPC transport protocols without heavyweight proxy dependencies.

Negative:
- Handlers become dependent on Connect envelope types and error encapsulation patterns.
- Service definitions must stay strictly synchronized with generated protocol contract interfaces.

## Alternatives

- Adoption of raw HTTP handlers with manual JSON encoding and decoding (rejected)
  Rejected because: Lacks schema-driven typed contracts, requires redundant parsing boilerplate across handlers, and increases risk of contract drift across client and server boundaries.
  When valid: Valid for third-party webhook receivers or simple ping endpoints that cannot adhere to RPC schemas.
- Adoption of standard gRPC runtime transport and stub generators (rejected)
  Rejected because: Introduces heavier runtime dependencies and mandates strict HTTP/2 protocol requirements that complicate deployment across heterogeneous network environments and edge proxies.
  When valid: Valid when communicating exclusively within an internal service mesh requiring pure gRPC transport framing.

## Risks

- Breaking changes between generated protocol schemas and Connect RPC handler method signatures.
  Mitigation: Perform lock-file version grounding before updates and enforce automated contract validation in continuous integration.
  Owner: Backend Engineering Team
- Premature context cancellation of asynchronous downstream reports upon client connection termination.
  Mitigation: Use explicit detached context wrappers with isolated timeouts when dispatching asynchronous tasks from request contexts.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implementations should register generated service handler interfaces onto HTTP routing trees using the standard Connect router registration functions.
- Handlers spawning asynchronous background tasks that exceed the lifecycle of the incoming RPC must derive detached context boundaries to prevent unintended cancellation.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository configuration and execute the static analysis suite across all service handler packages.
- Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints.

Accept when:
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

## Enforcement

- Verified by: Continuous integration pipelines executing schema compilation, static analysis, and interface verification suites.
- Verified by: Architectural review during pull request inspection confirming adherence to Connect RPC generic envelopes and error conventions.
- Violation handling: Pull requests introducing non-Connect endpoint handlers or bypassing Connect error code construction will fail automated checks and be blocked from merging.
- Violation handling: Violations identified during code review must be refactored to comply with standard Connect request, response, and error types.
- Exception process: Requests to expose endpoints outside the Connect RPC specification require documented architectural approval and designated exception justification in pull requests.