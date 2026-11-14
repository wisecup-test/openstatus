---
version: 2
name: fc7f6ccd-0c9e-4042-a0d0-8e4cba18b462-structured-context-aware-logging-via-log-package-applications-utilizing-log-package-use-structured
description: "Structured Context-Aware Logging via log Package: Applications Utilizing Log Package Use Structured"
metadata:
  actual:
    adr_id: fc7f6ccd-0c9e-4042-a0d0-8e4cba18b462
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Applications Utilizing Log Package Use Structured

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/railway-proxy`

Applies to:
- `**` (whole root)

## Context

- The proxy networking layer coordinates dial retries and server lifecycle events requiring detailed runtime diagnostics.
- Structured logging with contextual metadata is necessary to isolate connection failures and operational state transitions without string parsing.
- Context-aware loggers bind trace references to outbound error and lifecycle telemetry across concurrent goroutines.

## Problem Statement

Unstructured string logging during network transport execution and server lifecycle failures obscures critical diagnostic metadata, preventing automated error aggregation and contextual tracing across concurrent proxy operations.

## Decision

1. MUST: Applications utilizing the log package MUST use structured logging methods including log.Ctx for context-bound logging and leveled loggers with explicit field chaining for diagnostic telemetry.

## Policy Block

- MUST Applications utilizing the log package MUST use structured logging methods including log.Ctx for context-bound logging and leveled loggers with explicit field chaining for diagnostic telemetry.

In scope:
- Network transport retry routines and reverse proxy boundary handlers.
- Server lifecycle and asynchronous process supervisors.

Out of scope:
- In-memory data manipulation routines without network or IO side effects.
- Stateless transformation utilities not participating in request dispatch.

## Rationale

- Call-site evidence in proxy transport demonstrates that attaching contextual metadata such as retry counts and target URLs via chained builder methods facilitates automated log parsing.
- Context-bound logging via log.Ctx allows correlation of server lifecycle and request processing failures with parent runtime contexts.
- Structured error chaining preserves underlying diagnostic traces without error string mangling.

## Consequences

Positive:
- Machine-parsable log output facilitates automated querying and log aggregation across network operations.
- Contextual correlation through log.Ctx simplifies debugging of concurrent network transport failures.
- Standardized method chaining guarantees uniform key naming for critical operational attributes.

Negative:
- Increases coupling to the specific method chaining conventions of the adopted logging package.
- Imposes additional allocation overhead when constructing structured logging chains on hot network paths.
- Requires developers to pass context references consistently across all transport and server layers.

## Alternatives

- Standard library unformatted output without structured field chaining (rejected)
  Rejected because: Unstructured strings lack consistent machine-parsable fields, hindering automated log ingestion and tracing in proxy environments.
  When valid: Valid only in minimal CLI utilities where structured aggregation is unnecessary.
- Global context-free logger emitting direct JSON maps (rejected)
  Rejected because: Omitting context attachment prevents linking log emissions with request cancellation and distributed tracing metadata.
  When valid: Valid in standalone background workers that lack contextual request lifetimes.

## Risks

- Inconsistent field key naming across different modules causes fragmentation in log queries.
  Mitigation: Standardize field naming conventions for errors, attempts, and targets across all call sites.
  Owner: Engineering Team
- Context omission at call sites causes loss of trace correlation in operational logs.
  Mitigation: Enforce context propagation through interface contracts and static analysis checks.
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
- When constructing retry loops for transport handlers, instantiate structured log chains at the point of failure before scheduling backoff delays.
- Extract the logger from the request context using log.Ctx to ensure tracing identifiers are automatically attached to outgoing log messages.

## Continuation Context


Verify commands:
- Discover the project's static analysis and linting scripts from the repository configuration and execute them to verify structured logging adherence.
- Discover and run the project test suite to validate context propagation and logging execution during transport retries.

Accept when:
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines.
- Verified by: Peer code review verification of context propagation and structured field chaining.
- Violation handling: Pull requests containing unstructured or context-free logging in proxy pathways MUST be blocked until corrected.
- Exception process: Exceptions for performance-critical hot paths requiring zero-allocation logging MUST be documented and approved by the architecture review board.