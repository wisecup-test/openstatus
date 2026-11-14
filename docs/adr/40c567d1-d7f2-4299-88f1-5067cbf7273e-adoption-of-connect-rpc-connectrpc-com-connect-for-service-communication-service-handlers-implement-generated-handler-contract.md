---
version: 2
name: 40c567d1-d7f2-4299-88f1-5067cbf7273e-adoption-of-connect-rpc-connectrpc-com-connect-for-service-communication-service-handlers-implement-generated-handler-contract
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Handlers Implement Generated Handler Contract"
metadata:
  actual:
    adr_id: 40c567d1-d7f2-4299-88f1-5067cbf7273e
    status: accepted
    candidate_group_id: 96e6c8cc-2c78-4f76-b2f7-27ffc10a6a9c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - pkg/assertions/**
      - proto/private_location/v1/**
---

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Handlers Implement Generated Handler Contract

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `pkg/assertions/**`
- `proto/private_location/v1/**`

## Context

- The service architecture requires distributed probe components to communicate monitoring jobs and telemetry ingestion results over network boundaries.
- A remote procedure call framework is required to define strongly typed service contracts between probe runners and coordinator services.
- Code generation from contract definitions introduces generated client and handler interfaces for unary remote procedure calls.

## Problem Statement

Network telemetry services require standardized, type-safe protocols for dispatching health check configurations and ingesting telemetry results without tight coupling to underlying transport implementations.

## Decision

1. MUST: All service handlers MUST implement the generated service handler contract interfaces for remote procedure calls.

## Policy Block

- MUST All service handlers MUST implement the generated service handler contract interfaces for remote procedure calls.

In scope:
- Remote procedure call service contract definitions and client invocations across distributed probe components.
- Telemetry ingestion endpoints coordinating check results between runners and coordinators.

Out of scope:
- Local protocol checking utilities executing raw socket probes or local assertions.
- Internal string formatting and memory buffer operations.

## Rationale

- The intermediate representation identifies generated client and handler contracts utilizing unary remote procedure calls for telemetry exchange.
- Adopted RPC framework contracts decouple protocol transmission mechanisms from the execution of domain-specific health checks.
- Observed implementation contracts define explicit handler signatures and unary client call abstractions for structured data transfer.

## Consequences

Positive:
- Service interactions benefit from strongly typed request and response contracts across network boundaries.
- Decouples telemetry ingestion and check dispatching from underlying network transport details.
- Enables structured error and payload contracts across independent operational components.

Negative:
- Requires maintenance of protocol contract generation pipelines and runtime client dependencies.
- Introduces operational overhead for managing serialized request envelopes across distributed boundaries.

## Alternatives

- Ad-hoc REST API communication over raw HTTP clients (rejected)
  Rejected because: Lacks strongly typed compile-time contract definitions and uniform unary call abstractions across service components
  When valid: When integrating with legacy external webhooks that do not support standardized RPC contracts
- Standard gRPC with native HTTP/2 transport dependencies (rejected)
  Rejected because: Requires specialized transport infrastructure and heavier proxy overhead compared to Connect RPC protocol conventions
  When valid: When bi-directional streaming protocols are strictly required across low-latency internal infrastructure

## Risks

- Protocol contract drift between independent client implementations and backend service handlers.
  Mitigation: Enforce automated generation of client contracts from authoritative schema definitions during build phases.
  Owner: engineering team
- Single-file observation in repository evidence indicates potential isolated adoption rather than uniform codebase standard.
  Mitigation: Audit service communication patterns across all probe runner components before mandating framework adoption across all modules.
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
- Inspect the generated service interfaces to identify client construction patterns and unary method invocation signatures.
- Ensure context propagation is configured on all client calls to respect upstream deadlines and cancellation signals.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts from the repository configuration.
- Discover and run the test suite to verify client and handler contract compliance across service boundaries.

Accept when:
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

## Enforcement

- Verified by: Automated continuous integration checks executing compiler verification and test suites.
- Verified by: Peer code review auditing service client integration points.
- Violation handling: Pull requests bypassing the generated RPC client interfaces are blocked until contracts are adopted.
- Exception process: Architectural exceptions require review and documented approval by the lead architecture team.