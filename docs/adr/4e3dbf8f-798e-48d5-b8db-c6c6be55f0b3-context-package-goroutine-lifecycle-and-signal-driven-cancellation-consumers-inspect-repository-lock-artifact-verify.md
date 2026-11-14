---
version: 2
name: 4e3dbf8f-798e-48d5-b8db-c6c6be55f0b3-context-package-goroutine-lifecycle-and-signal-driven-cancellation-consumers-inspect-repository-lock-artifact-verify
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 4e3dbf8f-798e-48d5-b8db-c6c6be55f0b3
    status: accepted
    candidate_group_id: c6e113eb-84af-4600-96a4-243ccd32e981
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - cmd/private/**
      - cmd/server/**
---

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Consumers Inspect Repository Lock Artifact Verify

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `cmd/private/**`
- `cmd/server/**`

## Context

- Executable services within the project manage multiple concurrent operations, including HTTP servers and recurring background update managers.
- Uncoordinated process termination or unhandled server errors risk leaving background goroutines running or dropping in-flight network requests.
- Static analysis of entrypoint binaries demonstrates consistent use of context package cancellation triggered by signal capture channels and listener error traps.

## Problem Statement

Without a standardized concurrency lifecycle pattern, concurrent server processes risk orphan goroutines, stalled shutdown sequences, and inconsistent handling of termination signals or fatal runtime errors.

## Decision

1. MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of all runtime dependencies prior to implementing or extending concurrency coordination routines.

## Policy Block

- MUST Consumers MUST inspect the repository lock artifact to verify the exact resolved version of all runtime dependencies prior to implementing or extending concurrency coordination routines.

In scope:
- Service command entry points and long-running daemon processes managing concurrent network listeners or polling tasks.
- Background workers executing recurring updates or event processing.

Out of scope:
- Short-lived command-line utilities and batch scripts that execute synchronously without background goroutines.
- Stateless helper functions and pure domain logic operating without concurrent execution primitives.

Exceptions:
- EXP-17-001: A third-party client interface does not accept context instances and manages its own internal lifecycle.

## Rationale

- Multiplexing shutdown channel signals and listener errors directly into context cancellation provides deterministic teardown across all dependent subsystems.
- Checking context completion within background loop select blocks prevents goroutine leaks and allows in-flight operations to finish cleanly before exit.
- The pattern unifies lifecycle handling across diverse entry points using standard context propagation.

## Consequences

Positive:
- Guarantees deterministic, graceful shutdown across server listeners and worker loops upon process termination or critical failure.
- Prevents goroutine leakage and resource starvation caused by abandoned background loops.
- Provides consistent observability and error reporting during application shutdown.

Negative:
- Requires every asynchronous worker and network operation to explicitly receive and respect context cancellation.
- Adds coordination boilerplate around goroutine initialization and signal channel monitoring.

## Alternatives

- Immediate process exit upon termination signal (rejected)
  Rejected because: Causes abrupt termination of in-flight client requests, prevents socket cleanup, and leaves background tasks in an indeterminate state.
  When valid: Acceptable only in catastrophic, unrecoverable memory corruption scenarios.
- Custom shared atomic boolean flags for termination signaling (rejected)
  Rejected because: Lacks integration with standard I/O cancellation semantics and requires custom polling rather than channel multiplexing.
  When valid: Only when operating in constrained environments where channel allocations are prohibited.

## Risks

- Omission of context cancellation checks in new worker loops causing indefinitely blocked goroutines during shutdown.
  Mitigation: Enforce code review checklists and automated static analysis to verify channel selection on context completion in all long-running loops.
  Owner: engineering team
- Blocking cleanup handlers delaying shutdown beyond container orchestrator grace periods.
  Mitigation: Attach explicit timeouts to shutdown contexts to force termination if graceful drain exceeds operational thresholds.
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
- Construct root contexts with cancellation in the entrypoint function and pass them down into server structures and background loop routines.
- Ensure that error paths from server listeners and signal channel receivers execute cancellation callbacks exactly once.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute concurrency linting checks.
- Discover and run the automated test suite with race detection enabled using the project execution tool.

Accept when:
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and race detection tests.
- Verified by: Mandatory peer code review for all modifications to service entry points and background worker loops.
- Violation handling: Pull requests omitting context cancellation propagation or signal handling are blocked from merging.
- Violation handling: Detected goroutine leaks in testing environments trigger automated build failures.
- Exception process: Submit an architectural review request outlining technical constraints preventing context adoption and specifying fallback termination mechanisms.