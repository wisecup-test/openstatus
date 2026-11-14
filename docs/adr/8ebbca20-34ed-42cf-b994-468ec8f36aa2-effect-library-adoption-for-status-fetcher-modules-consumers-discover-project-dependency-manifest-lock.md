---
version: 2
name: 8ebbca20-34ed-42cf-b994-468ec8f36aa2-effect-library-adoption-for-status-fetcher-modules-consumers-discover-project-dependency-manifest-lock
description: "Effect Library Adoption for Status Fetcher Modules: Consumers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 8ebbca20-34ed-42cf-b994-468ec8f36aa2
    status: accepted
    candidate_group_id: a1ad617f-2763-424d-96a8-0b772df61636
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Effect Library Adoption for Status Fetcher Modules: Consumers Discover Project Dependency Manifest Lock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/status-fetcher`

Applies to:
- `src/fetchers/**`

## Context

- The status fetcher package implements ingestion modules across multiple external status providers and raw endpoints.
- Heterogeneous upstream providers present varied failure modes including network outages, schema deviations, and unexpected response structures.
- Codebase evidence demonstrates consistent adoption of Effect across status fetcher modules alongside shared fetch and utility abstractions.

## Problem Statement

Status fetcher implementations must poll varied third-party endpoints and handle complex failure scenarios uniformly without leaking unhandled exceptions or relying on inconsistent asynchronous error handling patterns across different provider adapters.

## Decision

1. MUST: Consumers MUST discover the project dependency manifest and lock artifact to resolve the exact pinned version of the Effect library prior to implementation.

## Policy Block

- MUST Consumers MUST discover the project dependency manifest and lock artifact to resolve the exact pinned version of the Effect library prior to implementation.

In scope:
- All status fetcher modules implementing external provider status ingestion or health check data collection.
- Asynchronous pipelines responsible for fetching, parsing, and normalizing provider response payloads.

Out of scope:
- Static configuration files and pure data transfer type declarations without asynchronous behavior.
- Standalone presentation layers or consumer interfaces consuming normalized status outputs.

## Rationale

- Static analysis demonstrates consistent imports of Effect across all status fetcher modules, establishing it as the standard library for effect orchestration in this domain.
- Effect provides a robust foundation for composing asynchronous operations with explicit error channels, ensuring provider integration failures are handled deterministically.

## Consequences

Positive:
- Standardizes asynchronous error handling and execution flow across all status fetcher implementations.
- Provides explicit, statically typed representation of operational failures during external service polling.
- Enhances composability when chaining HTTP requests, validation passes, and payload normalization steps.

Negative:
- Increases the learning curve for engineers unfamiliar with functional effect systems.
- Requires strict adherence to functional composition paradigms instead of standard imperative async patterns.

## Alternatives

- Plain native async/await with imperative try/catch blocks (rejected)
  Rejected because: Lacks structured error typing, composable effect handling, and standardized cancellation mechanisms across heterogenous status providers.
  When valid: Valid in small standalone scripts without complex error recovery or composable pipeline requirements.
- Reactive streaming libraries (rejected)
  Rejected because: Introduces unnecessary event stream and subscription overhead for single-request polling status fetcher operations.
  When valid: Valid when status data is pushed continuously over long-lived bidirectional socket connections.

## Risks

- Steep conceptual learning curve leading to inconsistent or incorrect usage of Effect combinators.
  Mitigation: Provide internal implementation guides and shared utility wrappers around common Effect idioms for provider polling.
  Owner: engineering team
- API surface variations across library releases introducing incompatibilities in effect pipeline composition.
  Mitigation: Perform rigorous version grounding against the project lockfile and validate API compatibility before adopting new runtime primitives.
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
- Encapsulate external provider status fetch sequences inside Effect generator or pipe workflows.
- Map boundary parsing and network communication errors into strongly typed domain error channels within the Effect program.

## Continuation Context


Verify commands:
- Discover the project build and typecheck script from the repository manifest and execute it against the affected packages.
- Discover the project test suite runner from the repository manifest and execute all unit and integration tests covering the status fetchers.

Accept when:
- All status fetcher modules compile without type errors under the resolved Effect library version.
- All status fetcher test suites execute and pass cleanly when evaluating effect execution paths.
- No unhandled Promise rejections or uncaught runtime exceptions escape fetcher execution boundaries.

## Enforcement

- Verified by: Automated CI build pipelines verifying type safety and test coverage.
- Verified by: Peer code review confirming proper encapsulation of asynchronous operations within Effect workflows.
- Violation handling: Pull requests containing raw unmanaged async executions or unhandled exception patterns in fetcher modules are blocked from merging.
- Violation handling: Violations identified in existing modules must be refactored to conform to Effect effect pipelines.
- Exception process: Submit an architectural deviation request detailing why Effect primitives cannot model the specific integration.
- Exception process: Obtain approval from the core engineering team prior to merging alternative async control flows.