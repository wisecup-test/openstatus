---
version: 2
name: 535ed417-9818-41fd-8a2c-f8ac21a499b4-structured-logging-with-log-slog-log-entries-include-domain-specific-context
description: "Structured Logging with log/slog: Log Entries Include Domain Specific Context"
metadata:
  actual:
    adr_id: 535ed417-9818-41fd-8a2c-f8ac21a499b4
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Log Entries Include Domain Specific Context

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`

## Context

- Server applications require structured operational visibility to monitor initialization state, component health, and background task execution.
- Unstructured text logging impairs automated ingestion, metric extraction, and indexed search across distributed monitoring environments.
- Handlers and asynchronous workers encounter transient errors during outbound dispatch and data persistence that require contextual metadata for root-cause analysis.
- Adopting a standardized structured logging interface enables field-level queryability while maintaining zero-allocation logging paths for performance-critical operational paths.

## Problem Statement

Without a standardized structured logging mechanism, operational logging across server initialization routines and background workers defaults to inconsistent, unstructured text streams. This lack of structure prevents downstream log aggregation systems from indexing critical contextual fields such as error descriptions, instance identifiers, and domain entity references, and complicates automated anomaly detection and operational debugging.

## Decision

1. SHOULD: Log entries SHOULD include domain-specific context attributes such as monitor identifiers and location identifiers when logging events within request handlers or background jobs.

## Policy Block

- SHOULD Log entries SHOULD include domain-specific context attributes such as monitor identifiers and location identifiers when logging events within request handlers or background jobs.

In scope:
- Server initialization and lifecycle event logging
- Background asynchronous job and worker dispatch failure reporting
- HTTP and RPC server request handler diagnostic logging

Out of scope:
- High-throughput metric time-series data captured via dedicated telemetry collectors
- Distributed tracing spans governed by dedicated context propagation protocols

## Rationale

- Evidence across server initialization and status forwarding demonstrates direct utilization of log/slog with structured key-value attributes for diagnostic output.
- Structured key-value pairs provide deterministic schemas for external log aggregators and analytics platforms without requiring brittle regex extraction.
- Standard library logging primitives reduce third-party dependency footprint while guaranteeing consistent operational interfaces across concurrent server operations.

## Consequences

Positive:
- Log entries provide uniform, structured key-value attributes that are directly ingestible and indexable by downstream monitoring systems.
- Decoupling message strings from dynamic attributes eliminates arbitrary string formatting overhead and improves log categorization.
- Error reports consistently capture contextual entity identifiers alongside error descriptions for faster incident remediation.

Negative:
- Developers must adhere to manual key naming conventions to prevent schema fragmentation across different server packages.
- Variadic key-value argument signatures lack compile-time type validation for balanced key-value pairings.

## Alternatives

- Unstructured standard library formatted print logging (rejected)
  Rejected because: Unstructured string formatting requires complex regular expressions for downstream analysis and lacks machine-readable contextual fields.
  When valid: Valid only in minimal command-line utility tools with no log ingestion pipeline requirements.
- Third-party structured logging frameworks (rejected)
  Rejected because: External logging dependencies introduce additional dependency supply-chain management overhead when standard library capabilities fulfill all operational logging requirements.
  When valid: Valid when specialized high-performance zero-allocation encoders or custom log routing hooks are strictly required.

## Risks

- Inconsistent attribute key naming across different modules causes fragmentation in log indexing queries.
  Mitigation: Establish documented attribute naming conventions and enforce key consistency through automated repository static analysis checks.
  Owner: engineering team
- Accidental logging of sensitive tokens or authentication secrets in structured attributes.
  Mitigation: Apply automated security secret scanning rules and code review policies prohibiting the emission of credentials.
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
- Developers should ensure that every structured log call maintains balanced key-value pairs following the constant message string.
- Log messages must remain constant and concise to serve as stable event identifiers across log aggregators.

## Continuation Context


Verify commands:
- test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Repository root verified; discover and execute project test scripts"
- test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Discover and execute project linter scripts for structured logging verification"

Accept when:
- All project static analysis and linting scripts pass with zero errors regarding structured logging calls.
- All automated test suites execute and pass successfully across all server packages.

## Enforcement

- Verified by: Continuous integration static analysis pipelines validating structured logging conventions.
- Verified by: Pull request peer review checking for balanced attribute key-value pairs and absence of string interpolation in log messages.
- Violation handling: Automated continuous integration checks reject pull requests containing unstructured logging or formatting violations.
- Violation handling: Code review comments flag unstructured log statements and require conversion to structured key-value attributes before merge.
- Exception process: Exceptions must be submitted via an architectural review issue detailing why structured logging is not feasible for the specific scenario.
- Exception process: Temporary exceptions require approval from the technical lead and must include a remediation deadline.