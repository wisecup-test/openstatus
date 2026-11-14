---
version: 2
name: f86f210a-9473-4ada-85b8-84e6294b1fca-gin-framework-request-scoped-context-storage-transient-request-scoped-state-stored-framework
description: "Gin Framework Request-Scoped Context Storage: Transient Request Scoped State Stored Framework"
metadata:
  actual:
    adr_id: f86f210a-9473-4ada-85b8-84e6294b1fca
    status: accepted
    candidate_group_id: 3cda31a0-dcc3-403b-8349-6d2b958c7ca4
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - handlers/**
      - pkg/tinybird/**
---

# Gin Framework Request-Scoped Context Storage: Transient Request Scoped State Stored Framework

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/private/**`
- `cmd/server/**`
- `handlers/**`
- `pkg/tinybird/**`

## Context

- Probe verification services require passing shared execution telemetry, correlation identifiers, and intermediate evaluation events between incoming HTTP request routing and protocol checker handlers.
- Explicitly passing telemetry payloads and request-scoped state through deep function parameter lists creates signature coupling and API bloat across protocol validation routines.
- In-memory request context storage provides a lightweight mechanism to attach, mutate, and inspect event records during the lifecycle of a probe request without external caching infrastructure.

## Problem Statement

Probe execution services need to propagate correlation IDs, probe metadata, and mutable event records across routing layers, retry policies, and protocol handlers without polluting functional parameter signatures or introducing external datastore latency for ephemeral request state.

## Decision

1. MUST_NOT: Transient request-scoped state stored in the framework context via c.Set MUST_NOT be used as a substitute for persistent storage across independent requests or outside the active request lifecycle.

## Policy Block

- MUST_NOT Transient request-scoped state stored in the framework context via c.Set MUST_NOT be used as a substitute for persistent storage across independent requests or outside the active request lifecycle.

In scope:
- HTTP server request routing and middleware layers
- Protocol checker handlers receiving framework request contexts
- Transient per-request telemetry and event state propagation

Out of scope:
- Cross-request persistent caching or data storage
- Outbound HTTP client background workers operating outside the incoming request lifecycle
- Inter-process event streaming or external datastore persistence

Exceptions:
- EXC-28-001: A background task runs asynchronously beyond the HTTP request lifecycle and requires detached context propagation

## Rationale

- Utilizing the web framework context key-value store localizes transient event tracking to the HTTP request lifecycle, ensuring memory is reclaimed immediately upon request termination.
- Centralizing request-scoped context propagation avoids modifying handler signatures whenever new telemetry fields or execution metadata are introduced.
- Eliminates dependency on external caching layers or distributed stores for data that only exists for the duration of a single probe evaluation.

## Consequences

Positive:
- Decouples protocol handler function parameters from transient orchestration and telemetry metadata
- Ensures request-scoped event state is automatically garbage collected when the request context terminates
- Provides a consistent, framework-native mechanism for middleware to inject correlation identifiers before handler execution

Negative:
- Sacrifices compile-time type safety due to untyped empty interface storage in context key-value maps
- Introduces the possibility of runtime panics if type assertions on retrieved context values fail
- Hides data dependencies between middleware and handlers through string-keyed context lookups

## Alternatives

- Explicit parameter passing of typed event and context structs through every handler signature (rejected)
  Rejected because: Adds significant boilerplate and requires updating every handler signature across protocols whenever request metadata evolves
  When valid: In small, static service architectures with strict compile-time interface enforcement and no middleware abstraction
- External distributed caching layer for ephemeral request state (rejected)
  Rejected because: Introduces network latency, infrastructure complexity, and unnecessary serialization overhead for data whose lifecycle does not exceed a single request
  When valid: When probe state must survive process restarts or be shared across geographically distributed nodes

## Risks

- Type assertion failures on values retrieved from context causing runtime handler panics
  Mitigation: Enforce safe type assertions using the comma-ok idiom and validate key presence before accessing context data
  Owner: Backend Engineering Team
- Context key collisions caused by decentralized string literal keys across packages
  Mitigation: Define all context keys as typed package constants within a centralized package
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
- Implement accessor helper functions that encapsulate framework context set and get methods with concrete type assertions, preventing handlers from performing direct unsafe casts.
- Ensure that all background routines launched from request handlers receive a detached context rather than referencing the ephemeral request context after response completion.

## Continuation Context


Verify commands:
- Discover and run the repository static analysis and linting scripts to verify type assertions and context key usages.
- Discover and run the project test suite to validate request handler context propagation and error handling.

Accept when:
- All unit and integration tests covering request handlers pass without context retrieval failures or nil pointer panics
- Static analysis checks pass with zero unhandled type assertion warnings on context values

## Enforcement

- Verified by: Automated continuous integration pipeline running test suites and static analysis linters
- Verified by: Peer code review verifying safe context extraction and adherence to centralized key constants
- Violation handling: Pull requests containing unhandled context type assertions or string literal keys will be blocked
- Violation handling: Runtime panics caused by context retrieval will trigger incident review and test coverage mandates
- Exception process: Submit an architectural exemption request to the engineering leads detailing why request context storage cannot be used
- Exception process: Document approved exceptions with clear lifetime and cleanup guarantees in the affected component documentation