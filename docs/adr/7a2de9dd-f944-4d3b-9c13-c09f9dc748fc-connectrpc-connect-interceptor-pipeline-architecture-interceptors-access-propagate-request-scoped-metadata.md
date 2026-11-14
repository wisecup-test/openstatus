---
version: 2
name: 7a2de9dd-f944-4d3b-9c13-c09f9dc748fc-connectrpc-connect-interceptor-pipeline-architecture-interceptors-access-propagate-request-scoped-metadata
description: "@connectrpc/connect Interceptor Pipeline Architecture: Interceptors Access Propagate Request Scoped Metadata"
metadata:
  actual:
    adr_id: 7a2de9dd-f944-4d3b-9c13-c09f9dc748fc
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Interceptors Access Propagate Request Scoped Metadata

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/rpc/interceptors/**`

## Context

- Distributed RPC architectures require coordinated enforcement of cross-cutting concerns including telemetry tracking, structured request logging, and normalized error handling across procedure invocations.
- Embedding observability and exception formatting directly into individual RPC service procedures causes duplicated code and divergent operational behavior across endpoints.
- The RPC routing infrastructure adopts @connectrpc/connect interceptors to provide a modular pipeline that inspects request metadata, accesses shared context values, and processes downstream procedure execution.
- Request-scoped metadata is propagated across interceptor boundaries using dedicated context values rather than mutating global runtime state.

## Problem Statement

RPC endpoint handlers require unified execution of cross-cutting behaviors, including contextual request identification, structured diagnostic logging, failure normalization, and analytics tracking. Scattering these operational concerns across individual RPC service implementations leads to inconsistent error responses, missing telemetry, and tight coupling between domain business logic and transport-level observability mechanisms.

## Decision

1. MUST: Interceptors MUST access and propagate request-scoped metadata exclusively through @connectrpc/connect request context values using strongly typed context keys.

## Policy Block

- MUST Interceptors MUST access and propagate request-scoped metadata exclusively through @connectrpc/connect request context values using strongly typed context keys.

In scope:
- All inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer
- Cross-cutting request lifecycle handlers, including authentication context resolution, diagnostic logging, analytics event dispatch, and error transformation

Out of scope:
- Domain business logic executed within standalone domain services or repository layers
- Transport protocols and endpoints not managed through the @connectrpc/connect routing pipeline

Exceptions:
- EXC-20-001: A high-throughput streaming RPC procedure requires specialized zero-allocation logging or stream-specific telemetry that bypasses standard interceptor overhead

## Rationale

- Centralizing cross-cutting operational concerns within @connectrpc/connect interceptors decouples transport observability and error mapping from core business procedures, ensuring uniform protocol compliance.
- Utilizing request context values provides a typed, scoped mechanism for sharing contextual metadata across interceptor chains and handlers without resorting to mutable global state.
- Isolating error translation within a dedicated interceptor prevents internal implementation details and unhandled runtime exceptions from leaking across the RPC boundary to external clients.

## Consequences

Positive:
- Provides consistent observability and structured logging across all RPC service procedures without duplicating boilerplate logic in individual handlers
- Standardizes error normalization and prevents raw internal failure details from escaping across the RPC boundary
- Maintains a clean architectural separation of concerns between transport infrastructure and domain application logic

Negative:
- Adds linear processing latency to every RPC invocation as each interceptor in the pipeline processes the request and response
- Increases diagnostic complexity when tracing asynchronous call stacks across multiple composed interceptors
- Couples cross-cutting middleware implementations to the @connectrpc/connect interceptor interface contract

## Alternatives

- Inline handler wrappers for cross-cutting logic (rejected)
  Rejected because: Wrapping individual RPC procedure handlers introduces repetitive boilerplate, increases the likelihood of omission, and causes inconsistent observability across endpoints
  When valid: In standalone applications containing only one or two isolated procedure endpoints
- Upstream HTTP transport middleware prior to the RPC router (rejected)
  Rejected because: Upstream HTTP transport middleware lacks visibility into decoded RPC context values, typed procedure definitions, and protocol-specific status codes
  When valid: For transport-level network concerns such as TLS termination, coarse IP filtering, or connection-level rate limiting prior to protocol dispatch

## Risks

- Uncaught exceptions within an interceptor could abort the request pipeline before error logging or response normalization executes
  Mitigation: Enforce defensive error handling within each interceptor and maintain a dedicated top-level error interceptor to capture unhandled rejections
  Owner: Backend Platform Team
- Slow external telemetry or analytics dispatches inside interceptors could increase RPC latency
  Mitigation: Dispatch analytics events asynchronously or buffer them in memory without awaiting non-critical external calls in the critical request path
  Owner: Backend Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct interceptors as standalone functions conforming to the @connectrpc/connect interceptor signature and export them from dedicated interceptor modules within the RPC route hierarchy.
- Ensure keys used with request context values are uniquely defined typed keys to eliminate key collision risks across disparate interceptors.
- Compose interceptors in the RPC router configuration in order of dependency, placing context extraction first, followed by logging, error handling, and outbound telemetry.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
- Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline

Accept when:
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and static analysis suites
- Verified by: Mandatory architectural peer review on all pull requests modifying or introducing RPC routes and interceptor chains
- Violation handling: Pull requests introducing inline cross-cutting logic or bypassing interceptors will be blocked from merging until refactored into modular interceptors
- Exception process: Submit an architectural exception request detailing specific latency or streaming constraints, requiring formal sign-off from the backend platform lead