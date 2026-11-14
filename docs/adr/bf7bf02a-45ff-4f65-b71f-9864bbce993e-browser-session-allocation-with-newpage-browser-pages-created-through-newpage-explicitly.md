---
version: 2
name: bf7bf02a-45ff-4f65-b71f-9864bbce993e-browser-session-allocation-with-newpage-browser-pages-created-through-newpage-explicitly
description: "Browser Session Allocation with newPage: Browser Pages Created Through Newpage Explicitly"
metadata:
  actual:
    adr_id: bf7bf02a-45ff-4f65-b71f-9864bbce993e
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Browser Pages Created Through Newpage Explicitly

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/screenshot-service`

Applies to:
- `src/**`

## Context

- Automated visual capture and end-to-end browser workflows require dedicated page contexts to load target URLs and extract rendering output.
- Rendering dynamic web targets within backend execution environments necessitates runtime process isolation to prevent memory leakage and state contamination.
- The service initiates browser contexts using the newPage method to handle visual processing tasks within a single execution unit.

## Problem Statement

Unmanaged browser session creation in backend services risks unbounded resource consumption, cross-session contamination, and runtime unreliability when handling dynamic web operations without formal context boundaries.

## Decision

1. SHOULD: Browser pages created through newPage SHOULD be explicitly closed in a finally block to release runtime resources.

## Policy Block

- SHOULD Browser pages created through newPage SHOULD be explicitly closed in a finally block to release runtime resources.

In scope:
- Services executing automated browser navigation, visual capture, or end-to-end rendering workflows.

Out of scope:
- Standard HTTP endpoints and data-layer services that do not invoke browser runtimes.

## Rationale

- Allocating distinct contexts via newPage isolates cookies, cache, and script state across consecutive tasks.
- Explicit page creation establishes determinism during external document navigation and rendering cycles.
- Encapsulating browser interactions within discrete page lifecycles allows controlled error recovery and targeted teardown.

## Consequences

Positive:
- Guarantees clean execution contexts for each navigation and rendering task.
- Prevents cross-session state contamination and cookie leakage across operations.
- Provides discrete failure domains for individual browser tasks.

Negative:
- Introduces latency and memory overhead for creating and tearing down browser pages.
- Requires rigorous teardown logic to prevent orphaned browser processes and memory leaks.

## Alternatives

- Reusing a single persistent browser page across all sequential tasks (rejected)
  Rejected because: Shared pages retain cookies, session storage, and DOM state that lead to test and execution contamination across tasks.
  When valid: Valid only in low-memory read-only benchmarks where session state isolation is strictly unnecessary.
- Spawning an entirely new browser process for every individual operation (rejected)
  Rejected because: Process initialization introduces prohibitive cold-start latency and CPU overhead compared to page-level isolation.
  When valid: Valid in untrusted multi-tenant execution sandboxes requiring absolute OS-level process boundaries.

## Risks

- Resource exhaustion caused by orphaned page contexts when unhandled exceptions bypass teardown logic.
  Mitigation: Enforce strict page closure within guaranteed finally blocks alongside runtime timeout guards.
  Owner: engineering team
- Excessive memory consumption under concurrent browser page allocations.
  Mitigation: Implement queue-based concurrency limits to cap the number of simultaneous active pages.
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
- Discover the browser automation client configured in the service manifest and verify the supported session lifecycle methods against resolved documentation.
- Wrap all newPage allocations in structured resource management constructs to ensure page teardown executes regardless of operation success or failure.

## Continuation Context


Verify commands:
- Discover the workspace test runner from the root configuration and execute the automated test suite.
- Discover the repository static analysis and linting script to check for unhandled asynchronous browser promises and resource leakage.

Accept when:
- All automated tests covering browser page allocation and teardown complete with zero failures.
- Static verification confirms that every page allocated through newPage has an associated closure call in its termination path.

## Enforcement

- Verified by: Continuous integration test execution and automated static analysis.
- Verified by: Peer code review verification of page lifecycle management.
- Violation handling: Pull requests lacking deterministic page teardown or isolated context allocation will be blocked.
- Exception process: Exceptions require documented justification of performance constraints reviewed and approved by the lead architect.