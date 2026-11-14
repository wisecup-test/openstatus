---
version: 2
name: ba72f6a6-33bf-4f22-a4f8-bc4edb3d779b-logtape-logtape-structured-logging-library-consumer-discover-project-dependency-manifest-inspect
description: "@logtape/logtape Structured Logging Library: Consumer Discover Project Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: ba72f6a6-33bf-4f22-a4f8-bc4edb3d779b
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Consumer Discover Project Dependency Manifest Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/**`

## Context

- Service components and background cron workflows require consistent structured telemetry for incident response and operational monitoring.
- Ad-hoc console output creates unparsed text lines that prevent automated querying and cross-service request correlation.
- Asynchronous background tasks and scheduled routines need a lightweight mechanism to propagate ambient execution context across task lifecycles.

## Problem Statement

Background workflows, scheduled tasks, and request handlers across the service layer require structured observability and unified context propagation, but disparate or unstructured logging mechanisms degrade incident troubleshooting, inhibit trace correlation, and clutter production log streams.

## Decision

1. MUST: The consumer MUST discover the project dependency manifest and inspect the resolution artifact to resolve and verify the exact locked version of @logtape/logtape prior to implementation.

## Policy Block

- MUST The consumer MUST discover the project dependency manifest and inspect the resolution artifact to resolve and verify the exact locked version of @logtape/logtape prior to implementation.

In scope:
- All service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

Out of scope:
- Client-side browser applications and isolated third-party library wrappers that do not execute within the service runtime.

## Rationale

- Evidence from eleven service files demonstrates widespread adoption of @logtape/logtape for operational logging, error tracking, and request auditing.
- The @logtape/logtape library provides lightweight, runtime-agnostic structured logging with category isolation and first-class context propagation.
- Integrating @logtape/otel allows canonical log records and telemetry data to stream seamlessly into OpenTelemetry collectors without architectural divergence.

## Consequences

Positive:
- Enforces uniform structured log emission with queryable key-value metadata across all background jobs and routes.
- Enables contextual request tracking and correlation across asynchronous execution boundaries using withContext.
- Facilitates interoperability with OpenTelemetry exporters via dedicated sink adapters.
- Prevents unformatted and fragmented text output from polluting runtime logs.

Negative:
- Developers must adhere to structured property schemas rather than writing ad-hoc string interpolations.
- Additional dependency surface introduced across all workflow and background execution services.
- Improper error object logging requires explicit property normalization to prevent missing stack traces or serialization failures.

## Alternatives

- Native runtime console methods (rejected)
  Rejected because: Unstructured console calls output raw text streams that lack hierarchical category routing, structured attribute querying, and asynchronous context propagation.
  When valid: Localized standalone debugging scripts where telemetry capture and centralized log indexing are unnecessary.
- Heavyweight legacy Node-specific logging engines (rejected)
  Rejected because: They require complex custom transport configurations and lack built-in runtime-agnostic asynchronous context bindings suitable for lightweight background workers.
  When valid: Legacy codebases already coupled to specialized transport plugins that cannot migrate to modern web standard APIs.

## Risks

- Logging raw Error objects without extraction may result in empty or uninformative log payloads depending on serialization behavior.
  Mitigation: Explicitly normalize error objects into plain object attributes before logging to ensure error message and stack properties are properly captured.
  Owner: Engineering Team
- Excessive logging of wide event payloads in high-frequency background cron jobs could inflate storage consumption.
  Mitigation: Apply sampling filters to high-throughput canonical log lines and configure category-level verbosity thresholds.
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
- Initialize loggers at module scope using hierarchical category arrays to provide fine-grained filtering capabilities.
- Ensure error instances are normalized into dedicated metadata fields containing error name, message, and stack trace before emitting error log events.

## Continuation Context


Verify commands:
- Discover the repository static analysis script from the dependency manifest and execute it to verify that all logger imports reference @logtape/logtape.
- Discover and execute the project test runner script to validate that structured logging and context propagation assertions pass.

Accept when:
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

## Enforcement

- Verified by: Automated continuous integration linting checks verifying logger imports and preventing standard console calls.
- Verified by: Pull request review by service maintainers verifying structured metadata schemas and context propagation.
- Violation handling: Pull request checks will fail if unapproved console calls or unstructured logging patterns are detected.
- Violation handling: Violations must be remediated to use @logtape/logtape structured log methods before code merges.
- Exception process: Exceptions require documented architectural review outlining why standard structured logging cannot be utilized.
- Exception process: Approved exceptions must be tracked with an expiration date and ticket identifier in the team task management system.