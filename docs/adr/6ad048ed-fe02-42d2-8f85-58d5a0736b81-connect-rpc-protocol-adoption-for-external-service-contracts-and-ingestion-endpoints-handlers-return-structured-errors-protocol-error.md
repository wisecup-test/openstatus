---
version: 2
name: 6ad048ed-fe02-42d2-8f85-58d5a0736b81-connect-rpc-protocol-adoption-for-external-service-contracts-and-ingestion-endpoints-handlers-return-structured-errors-protocol-error
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Return Structured Errors Protocol Error"
metadata:
  actual:
    adr_id: 6ad048ed-fe02-42d2-8f85-58d5a0736b81
    status: accepted
    candidate_group_id: 35c33ebb-f78e-4e67-899c-0be69ed767c1
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - proto/private_location/v1/**
---

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Return Structured Errors Protocol Error

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`
- `proto/private_location/v1/**`

## Context

- External distributed probe agents require reliable, strongly typed communication contracts with central ingestion servers for telemetry dispatch and configuration retrieval.
- Standardizing external communication protocols avoids diverging endpoint signatures, ad-hoc JSON payloads, and unstructured error response formats.
- The service architecture requires typed remote procedure call mechanisms that run natively over standard HTTP transports without complex gateway proxies.

## Problem Statement

External agent interactions require structured protocol contracts that ensure strict request and response schema conformance, uniform authentication parsing, and standardized error signaling without the operational overhead of bespoke transport proxies.

## Decision

1. MUST: Handlers MUST return structured errors using protocol error constructors with explicit status codes rather than naked error values.

## Policy Block

- MUST Handlers MUST return structured errors using protocol error constructors with explicit status codes rather than naked error values.

In scope:
- All external-facing remote procedure call interfaces connecting distributed agents to central ingestion endpoints.
- Server request handlers managing monitor synchronization and protocol telemetry ingestion.

Out of scope:
- Internal database persistence methods and domain repository access layers.
- Static health check endpoints and auxiliary HTTP routes operating outside the RPC contract definitions.

## Rationale

- Connect RPC (`connectrpc.com/connect`) provides typed schema-driven RPC over standard HTTP without requiring specialized proxies or dedicated HTTP/2 infrastructures.
- Consistent use of protocol request and response abstractions ensures end-to-end type safety between distributed probes and server handlers.
- Centralizing token authentication and structured error codes within protocol handlers eliminates unauthenticated state leaks across ingestion paths.

## Consequences

Positive:
- Enforces compile-time type safety and schema consistency between distributed remote probes and ingestion servers.
- Maintains native compatibility with standard HTTP routing, middleware, and reverse proxy infrastructure without dedicated gRPC translation layers.
- Standardizes status code handling and error serialization across all external ingestion and monitoring RPC methods.

Negative:
- Handlers depend on protocol-generated code artifacts that require schema compilation before implementation.
- Adoption of protocol-specific error structures requires mapping all internal errors to protocol status codes.
- Clients must implement protocol envelope semantics rather than generic unformatted HTTP calls.

## Alternatives

- Standard gRPC over HTTP/2 (rejected)
  Rejected because: Requires specialized proxy infrastructure and client networking configurations that complicate deployment in constrained remote agent environments.
  When valid: When bidirectional streaming performance across controlled private internal microservice meshes supersedes standard HTTP transport compatibility.
- Ad-hoc REST over JSON over HTTP (rejected)
  Rejected because: Lacks compile-time protocol buffer schema validation, typed client stub generation, and standardized cross-service error codes across language runtimes.
  When valid: When public endpoints must serve arbitrary third-party web browser clients requiring human-readable resource-oriented URL hierarchies.

## Risks

- Breaking schema changes between distributed external agents and backend ingestion endpoints.
  Mitigation: Maintain comprehensive automated contract test suites that validate serialization and deserialization across protocol updates.
  Owner: engineering team
- Unauthenticated requests executing downstream database queries if header extraction is omitted.
  Mitigation: Enforce mandatory token validation at the entrance of every protocol handler before database access.
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
- Generate protocol service contracts and client stubs from centralized schema definitions before implementing handler functions.
- Extract caller authentication credentials from incoming protocol request headers at the handler boundary prior to invoking internal query handlers.
- Map all downstream storage and operational errors into structured protocol error codes to prevent leaking internal database failure details to external callers.

## Continuation Context


Verify commands:
- Discover and execute the repository dependency lock verification script to validate locked protocol library versions.
- Discover and execute the repository test runner targeting all external protocol handler suites.
- Discover and execute the repository static analysis and linting suite to verify protocol contract compliance.

Accept when:
- All external RPC endpoints register handlers adhering to the generated service interface specifications.
- RPC handlers accept typed request containers, return typed response containers, and report errors using protocol status codes.
- Static contract and lock file verification checks pass without unauthenticated or untyped handler regressions.

## Enforcement

- Verified by: Automated static analysis verifying protocol contract implementations during continuous integration.
- Verified by: Peer architectural review of service interface definitions and handler registrations.
- Verified by: Contract validation suites asserting schema compliance and header token verification across all exposed endpoints.
- Violation handling: Continuous integration builds fail upon detecting untyped external endpoints or bypassed protocol request envelopes.
- Violation handling: Pull requests introducing non-compliant error codes or missing authentication checks are blocked from merging.
- Violation handling: Non-conforming service handlers must be refactored to wrap inputs and outputs in standard protocol contracts.
- Exception process: Submit an architectural revision proposal detailing why an endpoint cannot conform to the RPC contract generator.
- Exception process: Obtain approval from the technical architecture review board prior to exposing untyped or alternative transport endpoints.
- Exception process: Document transient protocol exceptions in the repository decision tracking registry with an assigned deprecation date.