---
version: 2
name: 8aac96af-ca9a-4742-a4ed-8ce83124e6dc-hono-routing-and-context-propagation-for-http-service-boundaries-route-definitions-declare-http-verbs-through
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Route Definitions Declare Http Verbs Through"
metadata:
  actual:
    adr_id: 8aac96af-ca9a-4742-a4ed-8ce83124e6dc
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Route Definitions Declare Http Verbs Through

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- HTTP service boundaries require consistent routing definitions, middleware composition, and scoped context propagation across decoupled handlers.
- Disparate route implementations risk fragmented request parsing, inconsistent response formatting, and uncoordinated security or telemetry data exchange.
- Adopting a unified HTTP framework establishes shared abstractions for request lifecycle management, typed context variables, and structured error responses across internal and public interfaces.

## Problem Statement

Without a standardized HTTP service boundary abstraction, route handlers and middleware independently manage request processing, context sharing, and response generation. This creates fragmentation in error handling, complicates telemetry instrumentation, and increases coupling across service entry points.

## Decision

1. MUST: Route definitions MUST declare HTTP verbs through router method invocations and return standardized JSON responses using the Hono Context json helper.

## Policy Block

- MUST Route definitions MUST declare HTTP verbs through router method invocations and return standardized JSON responses using the Hono Context json helper.

In scope:
- All HTTP service endpoints, route handlers, and middleware definitions exposed by the server application.

Out of scope:
- Non-HTTP background workers, database schema migrations, and offline batch processing routines.

## Rationale

- Evidence across 65 server route and middleware modules demonstrates standardized use of Hono router bindings and Context state propagation.
- Encapsulating request data flow within Hono Context ensures consistent downstream access to authentication credentials, telemetry traces, and workspace configurations.
- Declarative HTTP routing methods and structured context helpers provide uniform response ergonomics and unified error handling boundaries.

## Consequences

Positive:
- Uniform route handler signatures and lifecycle hooks simplify cross-cutting middleware integration.
- Deterministic request-scoped state management prevents data leaks between concurrent requests.
- Standardized JSON response helpers and parameter extraction promote consistent API contracts.

Negative:
- Coupling service boundaries to framework-specific context abstractions increases switching costs.
- Untyped or loosely typed context keys can introduce runtime lookup mismatches if data contracts drift between middleware and handlers.

## Alternatives

- Native runtime HTTP server handlers (rejected)
  Rejected because: Lacks integrated route matching, composable middleware pipelines, and ergonomic request-scoped context propagation.
  When valid: Valid in minimal execution environments with strict zero-dependency constraints.
- Process-global or thread-local state for request context (rejected)
  Rejected because: Introduces significant risk of state cross-contamination across concurrent requests and bypasses framework lifecycle guarantees.
  When valid: Valid in isolated procedural scripts executing single requests without concurrency.

## Risks

- Unchecked context property keys can result in undefined references at runtime.
  Mitigation: Enforce centralized type definitions for Context variable maps across all middleware and route handlers.
  Owner: engineering team
- Framework version divergence across packages could disrupt context compatibility.
  Mitigation: Pin framework dependencies at the workspace root and verify lock artifact alignment in automated checks.
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
- Define router instances per domain resource and compose them into the primary application router using prefix mounts.
- Initialize cross-cutting telemetry and authentication context in global middleware before routing requests to resource handlers.

## Continuation Context


Verify commands:
- Discover the test runner command from the repository configuration and execute the test suite to validate HTTP route definitions and status codes.
- Discover the static analysis and type checking command from the workspace manifest and execute it to ensure Hono Context variables adhere to expected type declarations.

Accept when:
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

## Enforcement

- Verified by: Automated continuous integration pipelines validating static types and running endpoint test suites.
- Verified by: Peer code reviews verifying adherence to framework context conventions and route structure.
- Violation handling: Pull requests introducing manual route dispatching or global state for request data are blocked until refactored.
- Exception process: Exceptions require documented architectural justification and formal sign-off from the technical lead.