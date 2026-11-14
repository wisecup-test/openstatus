---
version: 2
name: 6aaae4a0-c22a-4fb1-b756-d5ceffa270ff-openstatus-notification-base-external-client-integration-outbound-notification-dispatches-external-client-destinations
description: "@openstatus/notification-base External Client Integration: Outbound Notification Dispatches External Client Destinations"
metadata:
  actual:
    adr_id: 6aaae4a0-c22a-4fb1-b756-d5ceffa270ff
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Outbound Notification Dispatches External Client Destinations

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/channels/**`

## Context

- Outbound webhook and notification dispatchers communicate across external client boundaries to deliver event payloads to subscriber-provided endpoints.
- External endpoints exhibit variable network latency, intermittent availability, and potential hang conditions that risk worker pool exhaustion within internal services.
- The notification subsystem standardizes channel delivery through @openstatus/notification-base with schema validation, retry execution, and timeout enforcement.

## Problem Statement

External webhook delivery endpoints operated by third-party subscribers are inherently untrusted and subject to network unreliability, high latency, and unresponsive connections. Without standardized client boundary controls, unconstrained external requests can stall internal dispatch workers, leak sensitive subscriber credentials in error traces, and exhaust runtime resources.

## Decision

1. MUST: Outbound notification dispatches to external client destinations managed through @openstatus/notification-base MUST be executed via HTTP POST requests bound to explicit AbortSignal timeout instances to establish a deterministic execution deadline.

## Policy Block

- MUST Outbound notification dispatches to external client destinations managed through @openstatus/notification-base MUST be executed via HTTP POST requests bound to explicit AbortSignal timeout instances to establish a deterministic execution deadline.

In scope:
- All outbound HTTP requests dispatched to external client endpoints within notification delivery channels.
- Subscription webhook delivery, verification, and test notification dispatch routines.

Out of scope:
- Internal inter-service remote procedure calls governed by dedicated service mesh or internal RPC protocols.
- Read-only local file operations and database queries.

Exceptions:
- EXC-001: Diagnostic testing tools executing interactive single-step manual probes requiring extended debugging sessions.

## Rationale

- Enforcing explicit timeout signals on outbound HTTP dispatches prevents hung connections from blocking internal processing workers indefinitely.
- Integrating request execution with retry coordination pipelines ensures consistent resilience policies across different notification delivery channels.
- Validating channel configuration schemas prior to transmission avoids unnecessary outbound network traffic for malformed subscriptions.
- Redacting sensitive target identifiers in error logs protects subscriber endpoint security during transport failures.

## Consequences

Positive:
- Bounded execution duration for all external HTTP requests protects internal processing threads from resource starvation.
- Consistent retry and cancellation semantics ensure resilient event delivery across varied network conditions.
- Upfront schema validation catches configuration defects before outbound dispatch attempts.
- Redacted diagnostic logs prevent accidental exposure of sensitive webhook tokens and parameters.

Negative:
- Hard timeout limits terminate long-running responses from slow external endpoints that might otherwise have completed.
- Developers must explicitly manage signal lifecycles and configuration schemas for every new notification channel.
- Additional coordination overhead is introduced when wrapping dispatches in retry pipelines.

## Alternatives

- Unbounded HTTP dispatch without explicit abort signals (rejected)
  Rejected because: Leaving outbound client requests without timeouts causes worker thread exhaustion and pipeline blockage when external endpoints become unresponsive.
  When valid: Never valid in automated multi-tenant background notification pipelines.
- Relying solely on platform socket default timeouts (rejected)
  Rejected because: Platform socket defaults do not propagate cancellation across higher-level retry orchestration and application logic.
  When valid: Acceptable only for interactive diagnostic utilities in non-production environments.

## Risks

- External endpoints with elevated latency may experience premature dispatch cancellations due to overly aggressive timeout thresholds.
  Mitigation: Define configurable timeout durations per channel type and coordinate with retry strategies to handle transient latency spikes.
  Owner: notification engineering team
- Aborted requests may leave unhandled promise rejections or leaked abort listener handles if cancellation is improperly propagated.
  Mitigation: Ensure all channel clients tie signal lifecycles to their retry coordination workflows and cleanly handle abort exceptions.
  Owner: notification engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure timeout constants consistently across notification channels while allowing per-flavor payload customization.
- Pair input validation schemas directly with channel configuration parsing to reject invalid configurations before invoking network dispatch.

## Continuation Context


Verify commands:
- Discover the repository test runner from the project manifest and execute the test suite covering notification channel client dispatches.
- Discover and run the repository static analysis and type-checking scripts to verify type safety across external client boundaries.
- Discover and run the repository linting verification to confirm compliance with logging and error-redaction standards.

Accept when:
- All unit and integration test suites for notification channel clients pass with zero failures.
- Static type checking passes across all channel boundary implementations with zero errors.
- Outbound client requests verify timeout termination and signal propagation when external endpoints do not respond within configured thresholds.

## Enforcement

- Verified by: Automated continuous integration workflows validating static analysis, type checking, and unit test suites.
- Verified by: Mandatory peer code reviews for any modifications to notification channel dispatch logic.
- Violation handling: Continuous integration pipeline failure blocking pull request merging on missing timeout signals or unvalidated schemas.
- Violation handling: Rejection of changes during code review if external client invocations omit cancellation signal handling or credential redaction.
- Exception process: Formal exception request submitted to the architecture review board specifying technical necessity, compensatory controls, and sunset date.