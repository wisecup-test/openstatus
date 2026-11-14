---
version: 2
name: f1e59200-d985-4ddf-9669-24448ed027a6-console-api-for-diagnostic-and-operational-logging-before-introducing-any-external-logging-dependency
description: "Console API for Diagnostic and Operational Logging: Before Introducing Any External Logging Dependency"
metadata:
  actual:
    adr_id: f1e59200-d985-4ddf-9669-24448ed027a6
    status: accepted
    candidate_group_id: b26fa19d-bfc7-4c59-b7c9-13cbbd8e23ce
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
      - src/hooks/**
---

# Console API for Diagnostic and Operational Logging: Before Introducing Any External Logging Dependency

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `scripts/**`
- `src/hooks/**`

## Context

- Execution failures in build and synchronization scripts require immediate terminal visibility and guidance for remediation.
- Client-side browser hooks encounter runtime permission or support failures during asynchronous user interactions that require non-fatal diagnostic warning logs.
- The codebase currently relies on standard runtime console methods for operational warnings and terminal error output rather than an external structured logging framework.

## Problem Statement

Direct logging through runtime console methods is used across both build-time script execution and client-side interaction hooks for warning and error reporting without a unified logging abstraction or structured observability library.

## Decision

1. MUST: Before introducing any external logging dependency to replace built-in console invocations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version against official documentation.

## Policy Block

- MUST Before introducing any external logging dependency to replace built-in console invocations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version against official documentation.

In scope:
- Build automation scripts reporting execution prerequisites or configuration failures.
- Client-side hooks capturing non-fatal runtime interaction errors and fallback conditions.

Out of scope:
- Production server applications requiring structured JSON log streams or centralized log aggregation.
- Performance-critical rendering paths where synchronous console output introduces execution latency.

## Rationale

- Runtime console methods provide zero-dependency diagnostic reporting that functions consistently across both script execution environments and browser runtimes.
- Direct console invocations provide immediate actionable terminal feedback for script failures without adding external library overhead to lightweight utility packages.
- Because the observed pattern uses standard language runtime globals rather than an adopted third-party or internal logging framework, formal architectural governance is minimal.

## Consequences

Positive:
- Zero dependency footprint and zero configuration overhead across disparate runtime environments.
- Immediate visibility of script prerequisites and interactive failure modes in developer tooling.
- Consistent error and warning reporting using ubiquitous platform APIs.

Negative:
- Lack of structured metadata, correlation identifiers, or unified log level filtering.
- No centralized telemetry or log aggregation across deployed client applications.
- Potential for inconsistent log message formats across different modules.

## Alternatives

- Adoption of a centralized third-party structured logging framework (rejected)
  Rejected because: Introduces unnecessary package dependencies and configuration complexity for standalone build scripts and lightweight client utility hooks
  When valid: Valid when distributed services require centralized telemetry ingestion, log aggregation, and structured JSON outputs
- Silent failure suppression without logging (rejected)
  Rejected because: Hides actionable failure context from developers and prevents diagnosis of unsupported platform capabilities or prerequisite failures
  When valid: Valid when errors are fully expected, benign, and self-healing with zero diagnostic value

## Risks

- Unstructured console statements may inadvertently leak sensitive diagnostic context into production environments
  Mitigation: Enforce automated static analysis rules to flag and filter unauthorized console calls in production code paths
  Owner: engineering team
- Inconsistent log message schemas hinder programmatic parsing and troubleshooting
  Mitigation: Establish shared logging utilities or conventions when operational requirements demand uniform observability
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
- Ensure that error messages intended for terminal execution offer actionable next steps to resolve the reported condition.
- Verify that warning messages in client hooks provide contextual error objects to aid debugging without disrupting user interface state.

## Continuation Context


Verify commands:
- Discover the repository linter script from the project manifest and execute it to verify logging statements.
- Discover and run the project test runner to ensure diagnostic log statements do not cause unexpected test failures.

Accept when:
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines
- Verified by: Peer code review for all script and component modifications
- Violation handling: Static analysis failures blocking continuous integration builds
- Violation handling: Mandatory remediation of unauthorized or unredacted console statements prior to merge
- Exception process: Submission of an architectural deviation request approved by technical leads for specialized debugging or development tooling