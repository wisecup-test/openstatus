---
version: 2
name: 5e94c7b7-1b04-4135-9217-faac61c9ef51-github-com-rs-zerolog-structured-and-contextual-logging-adoption-operations-receiving-execution-context-retrieve-contextual
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Operations Receiving Execution Context Retrieve Contextual"
metadata:
  actual:
    adr_id: 5e94c7b7-1b04-4135-9217-faac61c9ef51
    status: accepted
    candidate_group_id: 69914c3b-8787-467a-9643-10591b9432bc
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - pkg/logger/**
      - pkg/otel/**
---

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Operations Receiving Execution Context Retrieve Contextual

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `pkg/logger/**`
- `pkg/otel/**`

## Context

- Network checking and telemetry collection operations require consistent machine-readable log output for diagnostics and metric aggregation.
- Concurrent service executions require context-bound logging to preserve request metadata across asynchronous call boundaries.
- Multiple service packages require a shared logging abstraction initialized through centralized configuration.

## Problem Statement

Application services and telemetry pipelines require high-throughput structured diagnostic logging that retains contextual trace data without introducing unstructured standard stream pollution or inconsistent formatting across independent packages.

## Decision

1. SHOULD: Operations receiving an execution context SHOULD retrieve contextual loggers via log.Ctx(ctx) to attach diagnostic attributes to error and event logs.

## Policy Block

- SHOULD Operations receiving an execution context SHOULD retrieve contextual loggers via log.Ctx(ctx) to attach diagnostic attributes to error and event logs.

In scope:
- All application services, network checking routines, telemetry pipelines, and configuration packages.

Out of scope:
- Third-party generated code and isolated standalone test fixtures that do not participate in application telemetry.

Exceptions:
- EXC-20-001: Pre-initialization fatal errors occur before the centralized logger configuration executes.

## Rationale

- The library github.com/rs/zerolog provides structured JSON output with zero-allocation characteristics essential for high-throughput network checking.
- Utilizing log.Ctx(ctx) allows contextual propagation of execution metadata without threading explicit logger references through every interface boundary.
- Centralized configuration through Configure establishes uniform log formatting and log levels across all dependent modules.

## Consequences

Positive:
- Standardized structured log output ensures uniform machine-readable telemetry across all checking and metric recording operations.
- Contextual logging enables correlation of diagnostics and errors with specific execution lifecycles.
- Centralized configuration standardizes log formatting and severity thresholds across application components.

Negative:
- Direct reliance on github.com/rs/zerolog types couples logging invocations across packages to a concrete third-party dependency.
- Developers must maintain discipline in propagating context objects to enable contextual log extraction.

## Alternatives

- Standard library unstructured logging (rejected)
  Rejected because: Unstructured logging lacks structured key-value attribution and requires manual formatting for context propagation across concurrent operations.
  When valid: Simple command-line utilities with minimal concurrency and no external telemetry ingestion pipeline.
- Direct uncontextualized global logger calls without contextual propagation (rejected)
  Rejected because: Bypassing contextual logger retrieval loses correlation IDs and execution metadata needed during concurrent check analysis.
  When valid: Process bootstrap phases before execution contexts are established.

## Risks

- Uninitialized logger configuration leading to default or silent log output during early boot failures.
  Mitigation: Ensure the centralized Configure call executes before any worker routines or network checks are scheduled.
  Owner: engineering team
- Contextual logger extraction falling back to a disabled or empty logger if context is unpopulated.
  Mitigation: Verify through static analysis that functions receiving context arguments extract the contextual logger rather than falling back to uncontextualized defaults.
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
- Invoke the centralized Configure routine during application startup to establish baseline logging settings before serving requests.
- Extract contextual loggers within service handlers using log.Ctx(ctx) to attach relevant diagnostic fields to execution errors.

## Continuation Context


Verify commands:
- Discover the repository static analysis and linting scripts from project configuration and run them to verify logger usage compliance.
- Discover and execute the automated test runner to ensure all logging and telemetry contracts validate successfully.

Accept when:
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

## Enforcement

- Verified by: Automated linting and static analysis rules executed during continuous integration.
- Verified by: Mandatory peer review on pull requests touching service logging and telemetry boundaries.
- Violation handling: Pull requests containing non-compliant logging calls or direct stream writing are blocked from merging.
- Violation handling: Violations detected in repository branches trigger immediate remediation tasks.
- Exception process: Submit an architectural change request documenting technical constraints that prevent structured logging usage.
- Exception process: Obtain explicit sign-off from the technical lead prior to merging unstructured logging implementations.