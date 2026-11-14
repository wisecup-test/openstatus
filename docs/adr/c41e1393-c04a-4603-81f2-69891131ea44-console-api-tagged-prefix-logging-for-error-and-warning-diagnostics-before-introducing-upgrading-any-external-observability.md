---
version: 2
name: c41e1393-c04a-4603-81f2-69891131ea44-console-api-tagged-prefix-logging-for-error-and-warning-diagnostics-before-introducing-upgrading-any-external-observability
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability"
metadata:
  actual:
    adr_id: c41e1393-c04a-4603-81f2-69891131ea44
    status: accepted
    candidate_group_id: 36d6268e-35a8-4f3e-88ed-9486d899106e
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
      - src/**
---

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `scripts/**`
- `src/**`

## Context

- The analytics client module requires operational observability to signal missing credentials and query evaluation failures without throwing unhandled exceptions to callers.
- Safe execution wrappers catch runtime errors and return fallback datasets, necessitating immediate diagnostic logging to prevent silent failures.
- Migration and backfill procedures require console feedback to notify operators of threshold overrides, abort triggers, and unmapped entities.

## Problem Statement

Client integrations and data backfill routines need a lightweight mechanism to communicate operational failures, missing configurations, and execution warnings without crashing calling processes or requiring heavy external logging infrastructure.

## Decision

1. MUST: Before introducing or upgrading any external observability or logging dependency, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version.

## Policy Block

- MUST Before introducing or upgrading any external observability or logging dependency, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version.

In scope:
- Client integration modules managing external analytics connectivity and safe query execution.
- Operational data backfill and migration scripts interacting with primary datastores.

Out of scope:
- High-throughput request tracing and distributed telemetry collection requiring centralized log collectors.
- User-facing client interface components and presentation layers.

## Rationale

- Using the standard Console API avoids introducing heavy third-party logging dependencies into a lightweight client integration package.
- Tagging log statements with bracketed component prefixes provides sufficient contextual attribution in container and serverless runtime log streams.
- Logging errors prior to returning empty fallback data ensures operational transparency while preserving fault-tolerant execution.

## Consequences

Positive:
- Zero external dependencies required for operational error and warning logging.
- Consistent bracketed tag prefixes allow straightforward log filtering in runtime log viewers.
- Safe wrappers provide visibility into swallowed query errors.

Negative:
- Lack of structured JSON output limits automated parsing and indexing in centralized observability platforms.
- Standard console methods lack runtime log level configuration, resulting in potential noise in non-development environments.

## Alternatives

- Adopt a structured third-party logging library with JSON serialization (rejected)
  Rejected because: Introducing a heavy structured logging framework increases bundle size and dependency overhead for a focused client integration package
  When valid: When centralized log ingestion requires structured schema enforcement and dynamic level filtering across all services
- Silent error suppression without console emission (rejected)
  Rejected because: Silent suppression conceals authentication failures and query execution bugs, hindering operator troubleshooting
  When valid: Never acceptable for diagnostic failures in client adapters

## Risks

- Excessive console output during persistent downstream outages may clutter runtime log streams
  Mitigation: Implement rate limiting or transition to an external structured logging provider if log volume exceeds operational limits
  Owner: Engineering Team
- Accidental inclusion of sensitive credentials or auth headers in error message payloads
  Mitigation: Sanitize error objects and enforce static analysis checks to prevent logging authorization variables
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
- Standardize bracketed domain prefixes across all client diagnostic messages to maintain uniform log message parsing.
- Ensure safe fallback routines attach the caught error object as a secondary parameter rather than concatenating it as a string to preserve stack traces.

## Continuation Context


Verify commands:
- Discover the repository static analysis and linting scripts from the dependency manifest and execute them to verify console logging compliance across client packages.
- Discover and execute the package test suite from the repository manifest to verify that fallback error handling emits diagnostic messages as expected.

Accept when:
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

## Enforcement

- Verified by: Automated code review and static analysis workflows configured in the continuous integration pipeline.
- Verified by: Peer code review verification of new client adapters and migration scripts.
- Violation handling: Pull requests containing untagged console calls or swallowed exceptions without logging must be blocked until remediated.
- Exception process: Exceptions must be reviewed and approved by the engineering team through documented pull request rationale.