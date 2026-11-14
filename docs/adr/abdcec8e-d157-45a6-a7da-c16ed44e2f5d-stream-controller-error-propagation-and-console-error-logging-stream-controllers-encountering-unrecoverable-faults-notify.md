---
version: 2
name: abdcec8e-d157-45a6-a7da-c16ed44e2f5d-stream-controller-error-propagation-and-console-error-logging-stream-controllers-encountering-unrecoverable-faults-notify
description: "Stream Controller Error Propagation and console.error Logging: Stream Controllers Encountering Unrecoverable Faults Notify"
metadata:
  actual:
    adr_id: abdcec8e-d157-45a6-a7da-c16ed44e2f5d
    status: accepted
    candidate_group_id: a9337cd3-1360-4529-9190-b2dc06c61223
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Stream Controller Error Propagation and console.error Logging: Stream Controllers Encountering Unrecoverable Faults Notify

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/utils`

Applies to:
- `src/**`

## Context

- Stream utilities require robust error handling to inform downstream consumers of stream lifecycle aborts.
- Operational diagnostics must be emitted when stream controllers encounter unexpected data processing or reading failures.
- The codebase exhibits direct pairing of console.error diagnostic logging with controller.error propagation in stream utility modules.

## Problem Statement

Stream pipelines encountering asynchronous or runtime failures risk silent stalling or unobserved dropouts if errors are swallowed or propagated without operational diagnostic recording. The system requires an immediate mechanism within stream processing utilities to signal consumer failure states while recording diagnostic traces.

## Decision

1. MUST: Stream controllers encountering unrecoverable stream faults MUST notify downstream consumers by invoking controller.error with the captured error instance.

## Policy Block

- MUST Stream controllers encountering unrecoverable stream faults MUST notify downstream consumers by invoking controller.error with the captured error instance.

In scope:
- Stream processing controllers and pipeline utilities handling asynchronous chunk transformation or source streaming.

Out of scope:
- Standard synchronous utility functions outside stream lifecycle pipelines.
- Higher-level application services where centralized telemetry or structured logging sinks are mandated.

## Rationale

- Invocations of console.error provide immediate visibility into stream pipeline aborts across developer consoles and execution runtimes.
- Signaling controller.error guarantees that downstream readers reject pending promises and release locked resources.
- Direct logging and controller error propagation in a single handler prevents resource leaks and silent pipeline stalls.

## Consequences

Positive:
- Immediate operational feedback on stream interruptions via standard error streams.
- Downstream stream readers receive explicit error notifications and terminate gracefully without hanging.
- Minimal overhead and zero additional external dependency requirements for stream utilities.

Negative:
- Direct console.error calls bypass structured log formatters and log aggregation pipelines.
- Output verbosity and log levels cannot be dynamically configured through central application logging configurations.

## Alternatives

- Silent error propagation via controller.error without local logging (rejected)
  Rejected because: Omitting local logging causes stream pipeline failures to disappear from diagnostic outputs if downstream consumers fail to inspect or log stream rejections.
  When valid: When stream consumers are guaranteed to perform comprehensive logging of all stream rejection events.
- Delegating stream error telemetry exclusively to an external logging framework (rejected)
  Rejected because: Introducing external telemetry framework dependencies into foundational stream utilities increases coupling and creates circular dependency hazards.
  When valid: In application-level orchestration services where dependency injection provides structured logging clients.

## Risks

- Unstructured diagnostic output from console.error may not integrate cleanly with automated log indexing systems.
  Mitigation: Standardize message prefixes across stream handlers to facilitate pattern matching in log aggregation.
  Owner: Engineering Team
- Sensitive error payloads passed directly to console.error could inadvertently expose internal state.
  Mitigation: Sanitize or limit error details before writing diagnostic messages to console streams.
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
- Ensure that any stream controller implementation consistently calls controller.error whenever a catch block catches an operational fault.
- Verify that error diagnostic strings provide context regarding the stream lifecycle stage without leaking sensitive parameters.

## Continuation Context


Verify commands:
- Discover the test runner from the project manifest and execute the test suite covering stream error propagation.
- Discover the linter configuration from the project repository and run static analysis checks against stream utility source directories.
- Execute the project build command discovered from repository configuration scripts to verify type integrity and error handling signatures.

Accept when:
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

## Enforcement

- Verified by: Automated test suites executing stream failure paths.
- Verified by: Pull request code reviews inspecting stream controller error handling.
- Violation handling: Pull requests omitting error propagation or diagnostic logging in stream handlers are blocked until addressed.
- Exception process: Architectural exceptions require approval from technical leads with documented justification for alternative error sinks.