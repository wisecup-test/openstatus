---
version: 2
name: a7e64a2e-2c1c-4fc6-97b4-32c3b7ba3199-checker-internal-module-adoption-for-http-synthetic-integration-probing-and-assertion-evaluation-http-integration-jobs-executing-probe-routines
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Http Integration Jobs Executing Probe Routines"
metadata:
  actual:
    adr_id: a7e64a2e-2c1c-4fc6-97b4-32c3b7ba3199
    status: accepted
    candidate_group_id: ca342d26-cc8f-4205-9a21-32db317001d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - handlers/**
      - pkg/job/**
      - pkg/scheduler/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Http Integration Jobs Executing Probe Routines

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `handlers/**`
- `pkg/job/**`
- `pkg/scheduler/**`
- `pkg/tinybird/**`
- `proto/private_location/v1/**`

## Context

- Synthetic integration testing requires dispatching active network requests to external endpoints and validating runtime availability, response timing, and response payloads against declarative assertions.
- Standardizing integration test execution across scheduled background jobs and immediate check handlers demands a centralized probe and assertion evaluation module rather than ad-hoc network calls.
- External HTTP dependencies exhibit transient network failures, necessitating structured retry policies with exponential backoff during integration verification passes.
- Evaluating integration assertions requires uniform inspection of HTTP response attributes including status codes, timing metrics, response headers, and response payload bodies.

## Problem Statement

Synthetic HTTP integration testing across distributed probing components requires a unified mechanism to execute external network requests, measure response latency, inspect payload bodies, and evaluate declarative assertion rules. Without an architectural standard governing integration probe execution, assertion evaluation logic becomes duplicated across scheduler workers and request handlers, creating inconsistent failure thresholds, fragmented retry semantics, and uncoordinated external client timeouts.

## Decision

1. SHOULD: HTTP integration jobs executing probe routines SHOULD record comprehensive timing metrics through Timing structures to enable latency percentile tracking alongside assertion outcomes.

## Policy Block

- SHOULD HTTP integration jobs executing probe routines SHOULD record comprehensive timing metrics through Timing structures to enable latency percentile tracking alongside assertion outcomes.

In scope:
- Components executing synthetic HTTP integration checks against external network endpoints
- Scheduler and worker modules evaluating declarative assertions against live HTTP responses
- Handlers processing synthetic monitoring probe requests and regional ping evaluations

Out of scope:
- In-memory unit tests validating pure business logic without network transport
- Internal service-to-service RPC dispatch that does not perform synthetic target assertion evaluation

## Rationale

- Centralizing synthetic integration probing within the checker module ensures consistent execution semantics, unified timing calculations, and standardized assertion evaluations across all probe executors.
- Separating network dispatch through Http from assertion evaluation through EvaluateHTTPAssertions allows assertion comparator logic to be tested and updated independently from HTTP transport implementations.
- Enforcing backoff.Retry with exponential backoff prevents transient network blips from generating false-positive integration failures while protecting external endpoints from aggressive retry storms.

## Consequences

Positive:
- Deterministic integration test assertion evaluation across scheduled and on-demand execution contexts.
- Resilient probe execution through standardized exponential backoff retry policies.
- Accurate latency and timing metrics captured uniformly alongside payload body assertion results.

Negative:
- Increased memory allocation overhead when buffering and reading HTTP response payload bodies for comparison.
- Additional execution latency introduced by exponential backoff retry cycles on failing remote endpoints.

## Alternatives

- Ad-hoc integration probe execution directly within each handler and scheduler worker (rejected)
  Rejected because: Duplicating network client setup, timeout management, and assertion logic leads to divergent assertion semantics and fragmented retry behaviors.
  When valid: Only viable in prototype micro-benchmarks with a single non-reusable probe requirement.
- External process invocation of standalone probe utilities (rejected)
  Rejected because: Spawning external operating system processes introduces substantial execution overhead and prevents programmatic inspection of response timing and payload bodies.
  When valid: Acceptable for black-box system smoke tests where native runtime integration is unavailable.

## Risks

- Response payload buffering for large payloads during integration assertion evaluation may cause memory spikes in high-concurrency worker pools.
  Mitigation: Impose strict upper bounds on read buffer sizes and configure client timeouts to drop oversized response bodies.
  Owner: Platform Engineering
- Excessive retry attempts during widespread third-party outages can saturate worker queues and delay subsequent scheduled integration checks.
  Mitigation: Cap maximum retry counts to bounded values using backoff.WithMaxRetries and backoff.WithMaxTries.
  Owner: Reliability Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project configuration settings to ensure synthetic probe timeouts and retry limits align with regional worker resource limits.
- Ensure response payload bodies read during assertion validation are closed and bounded to avoid connection leakage and unbounded memory allocation.

## Continuation Context


Verify commands:
- Discover and execute the project integration test suite to verify that probe dispatch and assertion evaluation contracts pass under simulated network conditions.
- Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints.

Accept when:
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

## Enforcement

- Verified by: Automated continuous integration test pipelines verifying assertion evaluation and probe execution contracts.
- Verified by: Peer code reviews auditing external network call sites for compliance with timeout and backoff policies.
- Violation handling: Merge requests introducing direct unmanaged HTTP calls without timeout or assertion coordination will be blocked.
- Violation handling: Non-compliant integration probe logic must be refactored to use the checker module contracts.
- Exception process: Exception requests must document the specific protocol or transport constraints preventing standard checker module adoption.
- Exception process: Temporary exemptions require sign-off from the reliability engineering lead with a tracked deprecation timeline.