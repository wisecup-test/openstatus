---
version: 2
name: 44634012-51fb-4f9e-b4d7-7ef7a0219472-adoption-of-logtape-logtape-for-structured-logging-and-contextual-wide-events-operational-log-entries-emitted-structured-property
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Operational Log Entries Emitted Structured Property"
metadata:
  actual:
    adr_id: 44634012-51fb-4f9e-b4d7-7ef7a0219472
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Operational Log Entries Emitted Structured Property

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- Backend services, RPC handlers, and routing layers require consistent operational visibility to monitor system health and troubleshoot failures.
- Unstructured log strings and ad-hoc runtime outputs prevent automated log parsing, centralized log aggregation, and metric derivation.
- Request lifecycle tracking requires associating operational log statements with execution context attributes such as identifiers, timing, and error details across asynchronous boundaries.

## Problem Statement

Inconsistent logging approaches across backend handlers and middleware lead to unstructured output, unindexed error payloads, and fragmented execution context, degrading operational observability during incident investigation.

## Decision

1. MUST: All operational log entries MUST be emitted using structured property payloads as key-value objects rather than string interpolation or string concatenation.

## Policy Block

- MUST All operational log entries MUST be emitted using structured property payloads as key-value objects rather than string interpolation or string concatenation.

In scope:
- All backend HTTP routes, RPC handlers, authentication and tracking middleware, and service initialization modules.
- Operational error reporting, lifecycle event tracking, and request telemetry within backend application services.

Out of scope:
- Ephemeral client-side browser user interfaces.
- Standalone utility scripts executed outside the application runtime environment.

Exceptions:
- EXC-46-001: Low-level bootstrapping scripts execute prior to logger initialization or during fatal runtime crashes where the logging subsystem fails.

## Rationale

- Adopting @logtape/logtape provides a unified logging interface that decouples log emission from specific sinks and formatting layers.
- Structured logging with property objects ensures operational data such as identifiers, status codes, and stack traces remain indexed and queryable in centralized log stores.
- Emitting canonical wide events per request lifecycle minimizes log volume while capturing comprehensive diagnostic context across asynchronous operations.

## Consequences

Positive:
- Standardizes structured operational logging across route handlers, RPC methods, and middleware components.
- Facilitates seamless export to telemetry backends without changing individual call sites.
- Enhances log searchability by indexing structured metadata fields for request identifiers, errors, and operational metrics.

Negative:
- Requires refactoring legacy unstructured console log statements across handlers to pass structured object payloads.
- Increases memory and processing overhead when formatting and serializing extensive context objects during high-throughput requests.

## Alternatives

- Unstructured runtime console logging across all handlers (rejected)
  Rejected because: Produces unindexed text streams that cannot be reliably parsed, queried, or correlated across asynchronous operations in centralized observability platforms.
  When valid: Permissible only in minimal single-file prototypes or early bootstrapping before logger initialization.
- Separate, disjoint logging libraries per route module or service (rejected)
  Rejected because: Creates incompatible log formats, fragmented configuration management, and inconsistent context propagation across service boundaries.
  When valid: Valid only when isolating completely independent polyglot services deployed outside the shared codebase.

## Risks

- Developers may inadvertently include sensitive data or secrets within structured logging payloads.
  Mitigation: Configure logging sanitizers and implement static analysis rules to flag sensitive attribute names before emission.
  Owner: Backend Engineering Team
- Disparate modules may continue using unstructured runtime outputs, fragmenting log ingestion.
  Mitigation: Enforce static linting rules that prohibit direct runtime console calls in server source directories.
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
- Configure category loggers at service boundaries to allow fine-grained level filtering and sink routing across distinct subsystems.
- Ensure all error logging passes serialized error objects with standard error message and stack trace fields rather than plain error strings.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting scripts to verify the absence of unapproved direct console logging across server source files.
- Discover and run the project test suite to verify structured log formatting and contextual log propagation across route handlers.

Accept when:
- Static analysis and linting verification checks pass with zero unapproved console logging invocations in server modules.
- All server handlers, middleware, and interceptors emit log events using the designated logging framework with structured object metadata payloads.

## Enforcement

- Verified by: Continuous integration static analysis checks enforcing logging rules.
- Verified by: Peer code reviews for all pull requests modifying server routing, middleware, or background tasks.
- Violation handling: Pull requests containing unstructured console logs or non-compliant log calls will fail automated checks and be blocked from merging until corrected.
- Exception process: Submit an architecture review request specifying the low-level bootstrapping constraint, subject to approval by the backend engineering team.