---
version: 2
name: c8968e5f-64e7-40f7-99df-bf3cf5006bd2-adopt-net-library-for-network-diagnostics-and-connectivity-probing-network-diagnostic-modules-not-introduce-third
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Network Diagnostic Modules Not Introduce Third"
metadata:
  actual:
    adr_id: c8968e5f-64e7-40f7-99df-bf3cf5006bd2
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Network Diagnostic Modules Not Introduce Third

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`

## Context

- Network diagnostic and checker modules require consistent mechanisms to validate connectivity and perform protocol inspections across targets.
- Multiple diagnostic components, including domain name resolution and transmission control protocol probes, require low-level transport operations.
- Standardizing network operations on a core networking library avoids divergent external dependency overhead while ensuring predictable operational semantics.

## Problem Statement

Network health checking components require low-level transport interactions to verify service availability and measure latency. Without an explicit architectural decision on networking libraries, implementations risk introducing disparate third-party networking clients with incompatible connection lifecycles, unaligned timeout behaviors, and unnecessary dependency footprints.

## Decision

1. MUST_NOT: Network diagnostic modules MUST NOT introduce third-party network client dependencies when transport and protocol capabilities are supported by the net library.

## Policy Block

- MUST_NOT Network diagnostic modules MUST NOT introduce third-party network client dependencies when transport and protocol capabilities are supported by the net library.

In scope:
- All service modules and components performing network diagnostic probes, connection checks, or protocol resolution.
- Diagnostic endpoints exposing health check and latency measurement contracts.

Out of scope:
- Higher-level application protocol clients requiring specialized application-layer protocol state machines.
- User-facing frontend presentation layers with no direct network socket operations.

Exceptions:
- EXC-20-001: A specialized network protocol or probe requires transport capabilities not supported by the core net library.

## Rationale

- Static analysis demonstrates that network probe components across DNS and TCP diagnostic modules consistently rely on the core net library.
- Direct use of the net library provides low-overhead network primitives without bringing transitive third-party dependencies into the diagnostic runtime.
- Standardizing on the net library ensures uniform socket lifecycle management and timeout controls across all health inspection components.

## Consequences

Positive:
- Zero external dependency overhead for core transport and network resolution operations.
- Consistent socket management, connection pooling, and cancellation across diagnostic probe modules.
- Clear separation between low-level transport probing and higher-level application logic.

Negative:
- Developers must manually construct protocol-level data structures and handle connection lifecycles.
- Detailed error discrimination requires explicit error type inspection rather than relying on high-level third-party client abstractions.

## Alternatives

- Adopt third-party network probing and pinging libraries (rejected)
  Rejected because: Third-party libraries introduce transitive dependency risks, potential licensing concerns, and unpredictable connection lifecycle semantics.
  When valid: When advanced protocol features or complex retry policies are required that exceed core library capabilities.
- Shell out to system diagnostic utilities (rejected)
  Rejected because: Invoking external system processes incurs heavy process spawn overhead, creates platform-specific dependencies, and introduces security attack surfaces.
  When valid: In standalone diagnostic scripts where native binary execution is required and runtime performance is non-critical.

## Risks

- Unbounded network operations causing diagnostic routines to hang indefinitely during target outages.
  Mitigation: Enforce mandatory context-driven timeouts on all network socket dial and resolution operations.
  Owner: Network Engineering Team
- Resource leaks from unclosed socket connections under high-frequency probe scenarios.
  Mitigation: Ensure all network connections and response streams are closed using deferred cleanup blocks.
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
- Ensure context deadlines are passed directly to dialers and resolver queries to maintain consistent timeout behavior across all probe routines.
- Encapsulate transport probe responses into structured return types that capture both latency metrics and error classifications.

## Continuation Context


Verify commands:
- discover_project_test_runner && run_project_tests
- discover_project_linter && run_project_linter

Accept when:
- Network diagnostic and connectivity probe routines pass all unit and integration test suites.
- Static code analysis confirms network probe implementations consume the net library without unapproved third-party networking wrappers.
- All network operations demonstrate deterministic timeout enforcement under simulated connection failure conditions.

## Enforcement

- Verified by: Automated CI validation pipelines running test suites and static analysis tools
- Verified by: Peer review during pull request inspection
- Violation handling: CI builds fail if unapproved third-party network transport packages are introduced in diagnostic modules.
- Violation handling: Pull requests violating timeout or cleanup requirements must be remediated prior to merge approval.
- Exception process: Submit an exception request detailing specific protocol limitations to the Architecture Review Board.
- Exception process: Obtain documented approval before incorporating third-party networking libraries.