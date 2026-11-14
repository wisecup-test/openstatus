---
version: 2
name: d30e970a-4c91-444c-8aec-6cc410d58b3b-hono-request-context-storage-for-ephemeral-state-propagation-route-handlers-retrieve-request-scoped-tenant
description: "Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Retrieve Request Scoped Tenant"
metadata:
  actual:
    adr_id: d30e970a-4c91-444c-8aec-6cc410d58b3b
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Retrieve Request Scoped Tenant

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- HTTP server endpoints require access to shared request-scoped state such as authenticated tenant data, client identification, and distributed tracing metrics.
- Re-querying primary datastores or repeatedly decoding authentication tokens within individual route handlers introduces redundant I/O overhead and increases latency.
- Decoupled middleware and routing components must share request-specific artifacts without mutating global application state or introducing deep parameter hierarchies.

## Problem Statement

HTTP request pipelines execute cross-cutting concerns across multiple middleware functions and domain route handlers, requiring an isolated mechanism to resolve, store, and propagate request-scoped state across handler boundaries without redundant primary datastore operations or unsafe global mutable variables.

## Decision

1. MUST: Route handlers MUST retrieve request-scoped tenant and authentication metadata through context getter methods instead of re-querying primary datastores.

## Policy Block

- MUST Route handlers MUST retrieve request-scoped tenant and authentication metadata through context getter methods instead of re-querying primary datastores.

In scope:
- HTTP server request processing pipelines including middleware functions and route handlers
- Cross-cutting request-scoped data propagation including authentication credentials, tenant identity, and request telemetry

Out of scope:
- Long-term data persistence spanning multiple independent HTTP requests
- Inter-service background job execution pipelines operating outside HTTP context dispatch

## Rationale

- Propagating resolved tenant and credential models through Hono Context eliminates redundant database access across nested handlers, optimizing execution performance across 68 analyzed server files.
- Context-bound state is strictly scoped to the active HTTP transaction lifecycle, preventing cross-request data leaks and concurrency bugs.
- Centralizing request state retrieval through uniform getter and setter methods decouples middleware execution from route implementation details.

## Consequences

Positive:
- Eliminates duplicate datastore reads for authentication and tenant records across the handler chain.
- Encapsulates request lifecycle state within an isolated transaction scope, preventing cross-tenant leakage.
- Standardizes request-scoped metadata access patterns across all API route handlers.

Negative:
- Creates an architectural dependency on the upstream middleware execution order to populate required context keys.
- Increases memory allocation per active HTTP transaction if large data structures are attached to the context.

## Alternatives

- Repeated primary datastore querying in individual route handlers (rejected)
  Rejected because: Introduces significant datastore I/O overhead and latency by repeatedly re-authenticating and re-fetching tenant records on every handler invocation within a single request.
  When valid: Valid only in standalone script utilities lacking an active HTTP middleware lifecycle.
- Parameter drilling through handler function signatures (rejected)
  Rejected because: Creates rigid coupling between framework routing boundaries and internal utility signatures, complicating middleware composition and endpoint definition.
  When valid: Valid within pure domain logic functions decoupled from HTTP routing.

## Risks

- Unset context keys accessed downstream can result in unexpected runtime undefined values if middleware ordering is misconfigured.
  Mitigation: Enforce strict middleware registration ordering and define type-safe context schemas with static type checks.
  Owner: engineering team
- Over-populating context variables with large data structures can increase memory overhead per active request.
  Mitigation: Restrict context storage to lightweight identifiers, validated authentication tokens, and essential tenant metadata.
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
- Define a centralized context variable interface when initializing the HTTP application to provide type safety across all setter and getter invocations.
- Sequence authentication and tracing middlewares at the head of the request processing chain before dispatching to endpoint handlers.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script from the project manifest to ensure context key-value definitions adhere to typed context bindings.
- Discover and execute the repository static analysis and linting scripts to verify that request-scoped state access strictly uses context getter and setter methods.
- Discover and run the project automated test suite to validate that middleware populates required context variables before downstream route handlers execute.

Accept when:
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

## Enforcement

- Verified by: Static type analysis and automated test suites in continuous integration
- Verified by: Architectural and peer code review of middleware and route handler implementations
- Violation handling: Pull requests containing direct datastore re-queries for established request metadata or untyped context mutations are rejected
- Exception process: Submit an architectural review request with justification when an endpoint requires custom request isolation outside framework context bindings