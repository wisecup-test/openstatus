---
version: 2
name: ea92aa97-73a9-43eb-b3f8-ced08f567665-recognition-runtime-invocation-via-recognition-start-before-integrating-third-party-dependencies-supporting
description: "Recognition Runtime Invocation via recognition.start: Before Integrating Third Party Dependencies Supporting"
metadata:
  actual:
    adr_id: ea92aa97-73a9-43eb-b3f8-ced08f567665
    status: accepted
    candidate_group_id: c15e1737-e236-4335-8e81-899024ca948f
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/nav/**
---

# Recognition Runtime Invocation via recognition.start: Before Integrating Third Party Dependencies Supporting

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/nav/**`

## Context

- A client-side navigation feedback component incorporates speech recognition capabilities using recognition.start within interactive UI state hooks.
- Static analysis identified the method invocation under the integration testing facet despite the absence of an explicit integration test runner or test harness in the evidence.
- Direct invocation of client-side hardware APIs without abstraction complicates automated test execution in headless environments.

## Problem Statement

Invoking browser recognition routines directly within UI components without a structured integration testing and mocking strategy risks test failures in automated CI pipelines that lack audio hardware devices.

## Decision

1. MUST: Before integrating third-party dependencies supporting UI interaction or validation routines, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency version.

## Policy Block

- MUST Before integrating third-party dependencies supporting UI interaction or validation routines, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency version.

In scope:
- Interactive client-side components utilizing recognition.start routines.
- Integration test suites targeting user interface feedback modules.

Out of scope:
- Server-rendered components and backend API route handlers.
- Non-interactive static UI components.

## Rationale

- Encapsulating recognition.start behind testable interfaces prevents CI pipeline failures caused by missing media devices in headless execution environments.
- Mocking recognition lifecycle events enables reliable verification of form validation and state transitions across UI components.

## Consequences

Positive:
- Automated integration test suites execute deterministically without external hardware dependencies.
- Error and boundary states in recognition lifecycles become testable within CI pipelines.

Negative:
- Additional mocking boilerplate is required for testing client components that use speech recognition.
- Synthetic test doubles may diverge from subtle browser-specific recognition API runtime behaviors.

## Alternatives

- Unmocked End-to-End Device Testing (rejected)
  Rejected because: Headless continuous integration environments lack audio hardware devices, causing consistent test failures.
  When valid: When performing manual verification on physical target devices.
- Centralized Cross-Cutting Audio Test Framework (deferred)
  Rejected because: The codebase currently exhibits recognition.start in only a single feedback component, making a full cross-cutting framework premature.
  When valid: When multiple distinct features across the application depend on complex voice and audio processing workflows.

## Risks

- Test mocks may fail to capture browser-specific recognition termination or permission denial behaviors.
  Mitigation: Supplement integration unit stubs with targeted manual acceptance verification on supported browser targets.
  Owner: Engineering Team
- Asynchronous recognition session lifecycles may introduce state leaks if not cleanly aborted on component unmount.
  Mitigation: Enforce cleanup routines within component effect hooks to stop or abort active recognition instances.
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
- Locate the testing configuration within the repository manifest to identify the standard integration test command and harness.
- Construct mock interfaces matching the recognition session event emitter API before running automated component tests.

## Continuation Context


Verify commands:
- Discover and run the repository integration test script from the project manifest to ensure client feedback components pass automated suites without hardware dependencies.
- Execute the project static analysis and linting script to validate that component lifecycle hooks correctly manage cleanup for active recognition sessions.

Accept when:
- All integration test suites covering recognition-dependent components pass in headless environments.
- Recognition lifecycle events trigger expected validation and state mutations without unhandled exceptions.

## Enforcement

- Verified by: Continuous integration automated test suite execution on pull requests.
- Verified by: Peer architectural code review for all new client interaction components.
- Violation handling: Pull requests containing unmocked hardware API calls that fail in headless CI environments are blocked from merging.
- Exception process: Architectural exceptions require written rationale and approval from the frontend engineering lead.