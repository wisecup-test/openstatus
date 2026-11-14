---
version: 2
name: 962b85bf-2a72-4d8c-8e9f-543a55ae1ba7-console-api-logging-developers-route-diagnostic-error-reporting-through
description: "Console API Logging: Developers Route Diagnostic Error Reporting Through"
metadata:
  actual:
    adr_id: 962b85bf-2a72-4d8c-8e9f-543a55ae1ba7
    status: accepted
    candidate_group_id: 0174bab9-9040-4ba3-9c11-4dcd869b8464
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/components/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Console API Logging: Developers Route Diagnostic Error Reporting Through

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/components/**`
- `src/content/**`
- `src/lib/**`
- `src/trpc/**`

## Context

- The codebase requires logging of operational warnings and runtime exceptions across serverless route handlers, scheduled background jobs, and data utility functions.
- Developers currently record diagnostic output and error states by directly invoking runtime console methods rather than importing an external structured logging framework.
- Static analysis indicates that runtime console invocations are distributed across multiple server endpoints and client interactions without standardized log schema enforcement.

## Problem Statement

Direct invocations of runtime console methods without an adopted logging framework or standardized log format result in unstructured log output, making automated log ingestion, severity filtering, and distributed tracing difficult across deployed environments.

## Decision

1. MUST: Developers MUST route diagnostic and error reporting through standardized runtime console methods until a formal structured logging client is adopted.

## Policy Block

- MUST Developers MUST route diagnostic and error reporting through standardized runtime console methods until a formal structured logging client is adopted.

In scope:
- All server-side route handlers, background routines, and utility functions within the project.

Out of scope:
- Client-side interactive user feedback components that display notification toasts.

## Rationale

- Direct use of runtime console methods introduces zero runtime dependency overhead and operates natively across edge and serverless runtimes.
- Subsystem prefixing provides basic contextual tracing in log aggregators without requiring an external instrumentation SDK.
- Low architectural confidence reflects that relying on built-in console methods is an ad-hoc practice rather than a dedicated logging architecture.

## Consequences

Positive:
- Eliminates bundle size overhead and dependency management complexity associated with external logging libraries.
- Works uniformly across all execution environments without requiring runtime-specific logging adapters.
- Provides immediate visibility of unhandled exceptions and warnings during execution.

Negative:
- Generates unstructured log streams that require custom parsing rules in centralized log aggregators.
- Lacks automatic log-level filtering in production environments, potentially leading to excessive log volume.
- Does not automatically correlate log statements with distributed trace identifiers or request contexts.

## Alternatives

- Adopt a dedicated structured logging library (deferred)
  Rejected because: Adding a third-party structured logging client introduces bundle overhead and configuration complexity across serverless runtimes that is not yet justified by current observability needs.
  When valid: Valid when log aggregation requires JSON-formatted structured payloads and dynamic log-level filtering.
- Suppress server-side diagnostic logging completely (rejected)
  Rejected because: Suppressing diagnostic logging prevents diagnosis of runtime failures and background job errors in production environments.
  When valid: Never valid for server-side error handling.

## Risks

- Unstructured log output may hinder root-cause analysis during high-volume production incidents.
  Mitigation: Enforce consistent subsystem prefixing and error serialization in all logging calls.
  Owner: engineering team
- Accidental logging of sensitive data such as authentication tokens or personally identifiable information.
  Mitigation: Implement pre-commit static analysis checks to detect token and secret references in log arguments.
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
- Standardize subsystem prefixes using the convention of bracketed component names at the start of log messages.
- Ensure error instances are passed directly as secondary arguments to preserve stack traces in runtime output.

## Continuation Context


Verify commands:
- Discover the repository test runner script from the project manifest and execute the test suite to verify log call execution.
- Discover the project static analysis and lint script from the dependency manifest and execute it to check for prohibited logging patterns.

Accept when:
- All unit and integration tests execute successfully without unhandled logging errors.
- Static analysis checks pass with zero violations regarding prohibited logging statements.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and test validation suites.
- Verified by: Peer code review verifying adherence to subsystem prefixing and sensitive data omission.
- Violation handling: Pull requests containing unformatted or sensitive log statements are blocked by code review and automated checks.
- Exception process: Exceptions must be submitted via architectural review request and approved by the lead maintainer with documented justification.