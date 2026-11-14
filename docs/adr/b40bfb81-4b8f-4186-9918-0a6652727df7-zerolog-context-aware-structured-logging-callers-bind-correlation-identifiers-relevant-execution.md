---
version: 2
name: b40bfb81-4b8f-4186-9918-0a6652727df7-zerolog-context-aware-structured-logging-callers-bind-correlation-identifiers-relevant-execution
description: "Zerolog Context-Aware Structured Logging: Callers Bind Correlation Identifiers Relevant Execution"
metadata:
  actual:
    adr_id: b40bfb81-4b8f-4186-9918-0a6652727df7
    status: accepted
    candidate_group_id: 47420a38-3bc2-4e9c-9bec-2e9a2aa8e81d
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/server/**
      - handlers/**
      - pkg/job/**
      - pkg/otel/**
      - pkg/tinybird/**
---

# Zerolog Context-Aware Structured Logging: Callers Bind Correlation Identifiers Relevant Execution

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/server/**`
- `handlers/**`
- `pkg/job/**`
- `pkg/otel/**`
- `pkg/tinybird/**`

## Context

- The checker services execute concurrent network probes across multiple protocols requiring detailed operational error capture under strict performance requirements.
- Handlers, asynchronous retry loops, and external telemetry clients receive execution contexts containing request lifecycles and metadata.
- The codebase adopts `github.com/rs/zerolog` and standardizes logger extraction through context utilities rather than passing loggers explicitly or using global output functions.

## Problem Statement

In concurrent monitoring and checker systems, tracking operational errors and execution events across distributed network probes, asynchronous retry loops, and HTTP handlers requires structured, correlated logging. Disjointed logging techniques or global uncontextualized loggers lose request correlation, introduce race conditions on global state, and complicate log aggregation in observability backends. The checker services require a unified, performant logging pattern that binds structured attributes directly to request and task contexts without cluttering API contracts.

## Decision

1. SHOULD: Callers SHOULD bind correlation identifiers and relevant execution metadata to the context before dispatching downstream tasks so that contextual logger instances automatically preserve operational traces.

## Policy Block

- SHOULD Callers SHOULD bind correlation identifiers and relevant execution metadata to the context before dispatching downstream tasks so that contextual logger instances automatically preserve operational traces.

In scope:
- Execution flows, request handlers, protocol checkers, and background routines that accept a context.
- Logging of operational events, execution status transitions, and runtime errors in services with contextual boundaries.

Out of scope:
- Pure utility transformations and computational routines that execute without context propagation.
- Early application initialization prior to context creation.

Exceptions:
- EXC-46-001: The process is executing startup initialization before a base context is constructed.

## Rationale

- Fourteen files across checker handlers, network probe implementations, background jobs, and telemetry packages consistently adopt `log.Ctx(ctx)` for error and event logging.
- Utilizing `log.Ctx(ctx)` enables seamless extraction of pre-configured structured loggers without mutating function signatures across public checker contracts.
- Structured chaining through `.Error()`, `.Err(err)`, and `.Msg(...)` provides standard JSON-formatted log output that integrates cleanly with log ingestion pipelines.

## Consequences

Positive:
- Ensures uniform, structured log records correlated with request lifecycles across diverse protocol checkers and handlers.
- Eliminates global mutable logger state and avoids contaminating function parameter signatures with logger instances.
- Provides efficient zero-allocation field chaining for high-frequency health checks and retry loops.

Negative:
- Requires strict discipline across all call paths to ensure active contexts are passed without omission.
- Invoking logging methods on contexts lacking pre-configured loggers falls back to default logger behavior without specialized contextual fields.

## Alternatives

- Standard uncontextualized logging or unstructured standard output formatting (rejected)
  Rejected because: Lacks request-scoped correlation, structured field metadata, and zero-allocation JSON formatting necessary for scalable observability.
  When valid: Valid only in standalone command-line utilities without asynchronous or distributed execution context.
- Explicit logger parameter propagation across all internal and public function signatures (rejected)
  Rejected because: Pollutes domain contracts and checker interfaces with infrastructure dependencies rather than leveraging existing context channels.
  When valid: Valid in systems where execution contexts are prohibited or unavailable.

## Risks

- Passing an uninitialized or nil context to logger extraction yields a disabled logger fallback that silently discards messages.
  Mitigation: Enforce non-nil context validation in entry point middleware and verify logger attachment on root contexts.
  Owner: Backend Engineering Team
- Contextual logging overhead under high-throughput health check execution causing excessive allocations if fields are duplicated.
  Mitigation: Derive contextual loggers once per request boundary and utilize zero-allocation field chaining methods.
  Owner: Observability Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Initialize root loggers with required baseline attributes during service startup and attach them to parent contexts prior to dispatching operational handlers.
- When handling operational errors within loops or retry callbacks, preserve the original error instance by passing it directly to the structured error logging method.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and linting script from the project repository configuration, then run it to verify that all logging calls adhere to contextual logger extraction rules.
- Discover the automated test suite runner from the project repository configuration, then run the test suite to ensure that logger invocations during execution flows pass validation.

Accept when:
- Static analysis and linting checks report zero violations of contextual logging conventions across all handlers, checkers, and service modules.
- Automated test suites complete with zero errors and confirm expected error logging behavior during simulated operational failures.

## Enforcement

- Verified by: Automated linting and static analysis checks integrated into the continuous integration pipeline.
- Verified by: Peer architecture and code review during pull request inspection.
- Violation handling: Pull requests containing uncontextualized log invocations in context-aware paths are blocked from merging.
- Violation handling: Flagged violations must be refactored to extract the logger via context and chain structured attributes.
- Exception process: Submit an architecture review request detailing why an execution path cannot receive or propagate a context.
- Exception process: Obtain approval from the observability or backend architecture leads before merging code utilizing alternative logging mechanisms.
- Exception process: Document the uncontextualized logging behavior in the component documentation.