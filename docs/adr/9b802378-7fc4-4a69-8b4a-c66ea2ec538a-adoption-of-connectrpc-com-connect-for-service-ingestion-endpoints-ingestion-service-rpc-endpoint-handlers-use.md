---
version: 2
name: 9b802378-7fc4-4a69-8b4a-c66ea2ec538a-adoption-of-connectrpc-com-connect-for-service-ingestion-endpoints-ingestion-service-rpc-endpoint-handlers-use
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Ingestion Service Rpc Endpoint Handlers Use"
metadata:
  actual:
    adr_id: 9b802378-7fc4-4a69-8b4a-c66ea2ec538a
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Ingestion Service Rpc Endpoint Handlers Use

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`

## Context

- The private location architecture requires consistent, typed RPC interfaces to ingest monitoring results across HTTP, DNS, and TCP protocols.
- Ingestion handlers must enforce caller authentication, validate request schemas, enrich operational context, and forward structured probe data to analytics datastores.
- The codebase adopted connectrpc.com/connect to unify RPC service contracts with standard context propagation and error reporting.

## Problem Statement

Disparate ingestion handlers across different network probe protocols risk diverging in authentication enforcement, schema validation, error reporting, and telemetry tagging. Without a standardized RPC framework, handlers incur repetitive boilerplate, inconsistent error status mappings, and fractured protocol support between HTTP and RPC clients.

## Decision

1. MUST: Ingestion service RPC endpoint handlers MUST use connectrpc.com/connect to define endpoint signatures with generic request and response wrappers.

## Policy Block

- MUST Ingestion service RPC endpoint handlers MUST use connectrpc.com/connect to define endpoint signatures with generic request and response wrappers.

In scope:
- Private location RPC ingestion service endpoints.
- Server handlers receiving probe data and metrics.

Out of scope:
- Internal utility functions and pure data transformations.
- Non-RPC HTTP services and external third-party webhooks.

## Rationale

- Adopting connectrpc.com/connect provides type-safe request and response encapsulation while supporting standard HTTP, gRPC, and Connect protocols.
- Consistent error constructors and status codes ensure uniform client-visible failure modes across all ingestion endpoints.
- Context propagation and header access integrate cleanly with downstream telemetry enrichment and authentication pipelines.

## Consequences

Positive:
- Establishes uniform RPC handler contracts across all ingestion protocol implementations.
- Standardizes status codes and error serialization for client interactions.
- Integrates seamlessly with generated protocol buffer message types and context propagation.

Negative:
- Tightly couples endpoint implementations to connectrpc.com/connect library abstractions.
- Requires maintaining generated protobuf definitions alongside handler implementations.

## Alternatives

- Standard Library HTTP Handlers (rejected)
  Rejected because: Lacks native typed protobuf integration, requiring manual serialization, deserialization, and custom error translation across ingestion endpoints.
  When valid: When building lightweight public endpoints that do not consume protobuf schemas or require multi-protocol RPC compatibility.
- Traditional gRPC Framework (rejected)
  Rejected because: Imposes heavier transport constraints and proxy requirements compared to the HTTP-native protocol support provided by ConnectRPC.
  When valid: When operating in pure backend environments requiring specialized transport streaming features without HTTP JSON compatibility.

## Risks

- Upstream changes in connectrpc.com/connect API specifications could require widespread handler signature updates.
  Mitigation: Pin resolved library versions through the repository lock artifact and verify API stability before version upgrades.
  Owner: engineering team
- Improper error code mapping may expose internal infrastructure details or misclassify client errors.
  Mitigation: Enforce standard error constructors and explicit status code classifications across all endpoint handlers.
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
- All RPC endpoint handlers follow a consistent execution order: authentication verification, payload validation, context enrichment, downstream ingestion dispatch, and status response construction.
- Telemetry contexts must be extracted from the incoming request context to maintain trace continuity throughout downstream asynchronous pipeline operations.

## Continuation Context


Verify commands:
- Discover the project test execution harness and run the test suite for all ingestion endpoint handlers.
- Discover the project code analysis linter to verify that all RPC handlers conform to ConnectRPC handler signatures.

Accept when:
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

## Enforcement

- Verified by: Automated test suites executing endpoint contract verifications in the continuous integration pipeline.
- Verified by: Peer code reviews confirming compliance with ConnectRPC handler signature conventions.
- Violation handling: Pull requests with non-conforming handler signatures or improper error handling are blocked from merging.
- Violation handling: Discrepancies identified during code review must be resolved prior to release.
- Exception process: Exceptions for non-RPC ingestion endpoints must be submitted to the architecture review team with documented technical justification.