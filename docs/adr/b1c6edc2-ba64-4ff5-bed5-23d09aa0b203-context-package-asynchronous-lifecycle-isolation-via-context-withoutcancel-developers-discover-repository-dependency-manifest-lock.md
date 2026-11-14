---
version: 2
name: b1c6edc2-ba64-4ff5-bed5-23d09aa0b203-context-package-asynchronous-lifecycle-isolation-via-context-withoutcancel-developers-discover-repository-dependency-manifest-lock
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Discover Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: b1c6edc2-ba64-4ff5-bed5-23d09aa0b203
    status: accepted
    candidate_group_id: 1700c304-66aa-4383-b313-bb2df6cb6fc3
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
---

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Discover Repository Dependency Manifest Lock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `cmd/server/**`
- `internal/server/**`

## Context

- Inbound request handlers frequently initiate asynchronous side-effect operations such as workflow status forwarding.
- Standard context hierarchies cancel child contexts immediately when incoming client connections terminate or time out.
- Premature cancellation of side-effect operations leads to dropped status updates and telemetry loss.
- Decoupled background execution requires isolated cancellation lifecycles while retaining context values and bounding execution duration.

## Problem Statement

HTTP request handling routines frequently initiate auxiliary background tasks such as status reporting to downstream services. Relying directly on the inbound request context causes these tasks to abort unexpectedly whenever clients disconnect or encounter timeouts. Conversely, dropping the request context completely discards valuable trace metadata and request identifiers. The system requires an architectural approach to isolate background execution from parent context cancellation while preserving request attributes and enforcing bounded resource lifecycles.

## Decision

1. MUST: Developers MUST discover the repository dependency manifest and lock artifact to verify the exact resolved runtime library version before using detached context APIs.

## Policy Block

- MUST Developers MUST discover the repository dependency manifest and lock artifact to verify the exact resolved runtime library version before using detached context APIs.

In scope:
- Asynchronous downstream status reporting, telemetry forwarding, and audit dispatch triggered within request handlers.
- Background operations that require access to parent context values but must survive parent cancellation.

Out of scope:
- Synchronous request processing where client responses depend directly on execution results.
- Standalone daemon processes or persistent workers unattached to incoming request lifecycles.

## Rationale

- Utilizing context.WithoutCancel severs the cancellation signal from the inbound request while preserving context values and metadata across asynchronous boundaries.
- Constraining detached contexts with context.WithTimeout prevents orphan background tasks from consuming system resources indefinitely.
- Combining detached contexts with graceful server shutdown ensures server processes do not exit abruptly while in-flight background operations are concluding.

## Consequences

Positive:
- Downstream status reporting and telemetry persist reliably regardless of premature client connection termination.
- Context values and diagnostic attributes remain accessible to asynchronous operations.
- Deterministic timeout limits prevent background routines from running indefinitely.

Negative:
- Server resources remain allocated to background tasks after client disconnects until timeout expiration.
- Engineers must rigorously ensure cancel functions are deferred to prevent context leaks.

## Alternatives

- Spawning background operations using context.Background() (rejected)
  Rejected because: Discarding the parent context drops all associated context values, request identifiers, and trace attributes.
  When valid: When initializing standalone daemon routines completely decoupled from incoming request scope.
- Synchronously awaiting all downstream workflow reporting in the primary request path (rejected)
  Rejected because: Synchronous blocking inflates client latency and permits downstream degradation to impact server responsiveness.
  When valid: When downstream processing is strictly required to validate or complete the immediate client response.

## Risks

- Unbounded background routine creation leading to memory exhaustion if timeouts are omitted.
  Mitigation: Enforce mandatory context.WithTimeout derivation and immediate deferred cancellation on all detached contexts.
  Owner: Engineering Team
- Abrupt process termination interrupting in-flight detached background tasks.
  Mitigation: Implement coordinated graceful shutdown routines that wait for active detached operations within their deadline window.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all detached goroutines handle errors locally and record failures to structured logging.
- Coordinate server shutdown handlers to allow active bounded detached operations sufficient window to complete.

## Continuation Context


Verify commands:
- echo "Discover and execute the repository test runner defined in the project build configuration"
- echo "Execute static analysis and linting checks discovered from the repository manifest"

Accept when:
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline.
- Verified by: Peer code review of server request handlers and concurrency routines.
- Violation handling: Pull requests missing timeout constraints or deferred cancellation on detached contexts are blocked.
- Violation handling: Violations identified during code review must be resolved prior to integration.
- Exception process: Exceptions require documented architectural justification and explicit review approval from technical leads.