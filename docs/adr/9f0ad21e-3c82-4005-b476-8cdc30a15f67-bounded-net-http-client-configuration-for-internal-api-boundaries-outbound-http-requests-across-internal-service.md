---
version: 2
name: 9f0ad21e-3c82-4005-b476-8cdc30a15f67-bounded-net-http-client-configuration-for-internal-api-boundaries-outbound-http-requests-across-internal-service
description: "Bounded net/http Client Configuration for Internal API Boundaries: Outbound Http Requests Across Internal Service"
metadata:
  actual:
    adr_id: 9f0ad21e-3c82-4005-b476-8cdc30a15f67
    status: accepted
    candidate_group_id: 38c879f3-bd47-4707-8de8-53d6496737aa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - handlers/**
      - pkg/job/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Bounded net/http Client Configuration for Internal API Boundaries: Outbound Http Requests Across Internal Service

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/private/**`
- `cmd/server/**`
- `handlers/**`
- `pkg/job/**`
- `pkg/tinybird/**`
- `proto/private_location/v1/**`

## Context

- Services and background execution jobs interact across internal and external network boundaries to execute health checks, export telemetry, and query platform APIs.
- Unbounded outbound network calls risk exhausting system socket descriptors and worker thread pools when remote endpoints degrade.
- Configuring dedicated HTTP client instances with explicit timeout limits isolates operational boundaries and enforces resilient request dispatching.

## Problem Statement

Outbound HTTP interactions across service boundaries and monitoring tasks require deterministic resource management and resilience against hanging network connections. Relying on default unconfigured clients risks unbound goroutine accumulation, resource leaks, and lack of timeout isolation between distinct operational workloads.

## Decision

1. MUST: Outbound HTTP requests across all internal API and service integration boundaries MUST be executed using explicitly instantiated net/http Client instances configured with bounded timeouts rather than the default package-level client.

## Policy Block

- MUST Outbound HTTP requests across all internal API and service integration boundaries MUST be executed using explicitly instantiated net/http Client instances configured with bounded timeouts rather than the default package-level client.

In scope:
- All outbound HTTP client integrations communicating with internal API endpoints, monitoring targets, telemetry ingest services, and platform endpoints.
- Background jobs and request handlers initiating outbound network calls.

Out of scope:
- Inbound HTTP request routing and server listener management.
- Raw socket connections operating outside HTTP protocols.

## Rationale

- Instantiating dedicated net/http Client structs with explicit Timeout values prevents stalled network operations from locking worker routines indefinitely.
- Standardizing request execution via Do ensures uniform request header decoration, authorization token management, and context propagation.
- Combining explicit timeouts with exponential backoff retry policies guarantees graceful degradation when downstream internal services experience transient disruptions.

## Consequences

Positive:
- Guarantees deterministic resource recovery by preventing unconstrained connection hangs.
- Provides isolated timeout budgets calibrated to the operational characteristics of each downstream service.
- Ensures consistent context cancellation and authorization header injection across integration boundaries.

Negative:
- Introduces configuration boilerplate across packages that dispatch network requests.
- Improperly calibrated timeout limits can prematurely terminate valid long-running operations.

## Alternatives

- Rely on global package-level default HTTP client instances for outbound calls (rejected)
  Rejected because: Default client instances lack timeout boundaries, risking thread pool starvation and resource leakage during network degradation
  When valid: Short-lived diagnostic utilities where process lifecycle guarantees termination
- Share a single static HTTP client instance across all internal and external communication boundaries (rejected)
  Rejected because: Distinct operational targets require disparate timeout budgets and retry behaviors that cannot be satisfied by a single configuration
  When valid: Homogeneous microservices where all downstream targets share identical latency SLAs

## Risks

- Aggressively low timeout configurations can induce false-positive failures under transient network congestion
  Mitigation: Calibrate timeout durations using workload-specific configuration settings and pair with exponential backoff retries
  Owner: engineering team
- Neglecting response body closure on client Do responses leaks underlying network socket resources
  Mitigation: Enforce static analysis lint checks ensuring deferred response body closure following successful client dispatch
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
- Construct net/http Client instances with explicit Timeout durations calibrated to the specific operational boundary before issuing Do requests.
- Ensure every request object attaches context awareness with deadline propagation and that all response bodies are consistently closed after consumption.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute the test suite covering client boundaries.
- Discover and run the project static analysis and linting suite to check for unconfigured HTTP client usages.

Accept when:
- All outbound HTTP client instantiations declare explicit timeout properties and pass automated tests.
- No instances of unconfigured package-level default HTTP client calls exist across integration packages.
- Verification suites confirm bounded execution and context propagation across client boundaries.

## Enforcement

- Verified by: Automated continuous integration test suite execution.
- Verified by: Static analysis checks identifying unconfigured HTTP client references.
- Verified by: Peer review during architectural pull request inspection.
- Violation handling: Pull requests with unconfigured or default HTTP client calls will be blocked at integration testing.
- Violation handling: Unbounded client references detected in audit require immediate remediation to introduce timeout bounds.
- Exception process: Exceptions require architectural review documenting why customized socket transport parameters are necessary.