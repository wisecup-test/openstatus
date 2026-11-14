---
version: 2
name: 360d633e-69ca-44bb-a7e0-85986d73d983-adoption-of-context-module-for-lifecycle-and-cancellation-management-consumer-inspect-project-dependency-manifest-lock
description: "Adoption of Context Module for Lifecycle and Cancellation Management: Consumer Inspect Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 360d633e-69ca-44bb-a7e0-85986d73d983
    status: accepted
    candidate_group_id: 0d6c9b0f-e1bf-4d13-b206-45625d6c1b76
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - pkg/job/**
      - pkg/otel/**
      - pkg/scheduler/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Adoption of Context Module for Lifecycle and Cancellation Management: Consumer Inspect Project Dependency Manifest Lock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/private/**`
- `cmd/server/**`
- `pkg/job/**`
- `pkg/otel/**`
- `pkg/scheduler/**`
- `pkg/tinybird/**`
- `proto/private_location/v1/**`

## Context

- Network probing routines, scheduler tasks, RPC service clients, and resilience retry loops require bounded execution and cooperative cancellation to prevent resource exhaustion.
- Components across job execution, monitoring, telemetry recording, and server daemons must coordinate lifecycle events such as graceful shutdowns and timeout handling.
- A standardized mechanism is required across all subsystem boundaries to propagate deadlines, cancellations, and execution metadata without coupling components to custom signaling protocols.

## Problem Statement

Asynchronous operations, network probers, background scheduler tasks, and RPC invocations can hang or orphan resources if external endpoints become unresponsive or parent execution units terminate abruptly. Without a consistent, unified lifecycle propagation mechanism across all modules, individual components risk resource leakage, goroutine accumulation, and inconsistent timeout behavior.

## Decision

1. MUST: The consumer MUST inspect the project dependency manifest and lock artifact to verify the active runtime environment and standard library compatibility before implementing or modifying context-managed components.

## Policy Block

- MUST The consumer MUST inspect the project dependency manifest and lock artifact to verify the active runtime environment and standard library compatibility before implementing or modifying context-managed components.

In scope:
- All public contracts, interfaces, and function signatures that perform network I/O, protocol probing, or external service interactions
- Background schedulers, daemon loops, and asynchronous job runners requiring cooperative termination
- RPC service clients, server handlers, and resilience retry wrappers

Out of scope:
- Pure computational helper routines and synchronous in-memory data transformation functions without external boundaries
- Static configuration parsers that operate entirely on local memory without external I/O or concurrency

Exceptions:
- EXC-20-001: Decoupled asynchronous telemetry or logging flushes that must proceed to completion despite parent operation cancellation

## Rationale

- Standardizing on the context module enables unified lifecycle coordination across heterogeneous protocols including HTTP, DNS, TCP, and RPC without introducing external dependencies.
- Cooperative cancellation guarantees that when a parent request or shutdown signal triggers, downstream network calls and retry loops terminate immediately.
- Adopting context passing as the first parameter adheres to idiomatic practices within the language ecosystem, facilitating seamless integration with standard libraries and external resilience frameworks.

## Consequences

Positive:
- Guarantees deterministic, cascading cancellation across all concurrent jobs, network clients, and background scheduler routines.
- Eliminates orphaned worker routines and prevents resource exhaustion under heavy network timeouts or external service degradation.
- Provides standardized deadline and timeout propagation compatible across HTTP, DNS, TCP, and RPC protocols.

Negative:
- Explicit parameter threading requires passing the context instance through every intermediate layer in the call graph.
- Improper handling such as forgetting to invoke deferred cancellation functions can lead to subtle context and timer leaks.

## Alternatives

- Bespoke channel-based cancellation and timeout signaling (rejected)
  Rejected because: Custom cancellation channels introduce non-standard signatures, lack automatic deadline propagation across multi-tiered calls, and fail to integrate with standard network clients and RPC frameworks.
  When valid: Low-level primitive algorithms where context struct allocations introduce unacceptable profiling overhead.
- Unbounded synchronous execution relying exclusively on client socket timeouts (rejected)
  Rejected because: Fails to propagate cascading cancellations when parent routines or callers abort, causing resource starvation and orphaned background worker executions.
  When valid: Single-shot command-line scripts executing linear workflows without concurrency or background daemons.

## Risks

- Context leaks caused by omitting the cancellation function invocation on derived contexts with timeouts.
  Mitigation: Enforce static analysis checks requiring deferred cancellation calls immediately after context creation and review timeout derivations.
  Owner: Backend Engineering Team
- Unresponsive worker loops failing to check context cancellation channels during extended operations.
  Mitigation: Implement select blocks handling cancellation signals in all long-running loops and validate prompt termination with automated tests.
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
- Derive child contexts with explicit deadlines or timeouts at the initiation of network operations and immediately schedule deferred cancellation.
- Ensure long-running scheduler tickers and message ingestion loops select on context completion channels to guarantee prompt response to shutdown signals.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting suite to verify context parameter ordering and deferred cancellation compliance.
- Discover and execute the automated test suite with race detection enabled to confirm cooperative cancellation and goroutine termination under timeout conditions.

Accept when:
- All static analysis checks pass with zero reported violations regarding context parameter order or uninvoked cancellation functions.
- Automated test suites pass with race detection enabled, validating that asynchronous workers and network requests terminate immediately upon context cancellation without leaking concurrent routines.

## Enforcement

- Verified by: Automated static analysis and linting rules enforced during continuous integration.
- Verified by: Automated unit and integration test suites executed with race detection.
- Verified by: Peer code review for all interface definitions and asynchronous worker loops.
- Violation handling: Pull requests violating context parameter placement or cancellation handling are blocked in automated continuous integration.
- Violation handling: Detected context leaks or unhandled cancellation loops must be remedied before deployment.
- Exception process: Submit an architecture review request detailing the necessity for context decoupling or custom lifecycle management.
- Exception process: Obtain approval from the lead architecture reviewer before merging non-standard lifecycle handling.
- Exception process: Document the exception with explanatory comments directly at the call site.