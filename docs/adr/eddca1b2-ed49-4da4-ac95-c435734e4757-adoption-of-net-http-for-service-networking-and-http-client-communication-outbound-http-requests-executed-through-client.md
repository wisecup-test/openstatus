---
version: 2
name: eddca1b2-ed49-4da4-ac95-c435734e4757-adoption-of-net-http-for-service-networking-and-http-client-communication-outbound-http-requests-executed-through-client
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Outbound Http Requests Executed Through Client"
metadata:
  actual:
    adr_id: eddca1b2-ed49-4da4-ac95-c435734e4757
    status: accepted
    candidate_group_id: 5f922c0d-ec98-41d5-bfbf-3b766b41eda2
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - cmd/private/**
      - handlers/**
      - pkg/job/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Adoption of net/http for Service Networking and HTTP Client Communication: Outbound Http Requests Executed Through Client

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `cmd/private/**`
- `handlers/**`
- `pkg/job/**`
- `pkg/tinybird/**`
- `proto/private_location/v1/**`

## Context

- Network probe execution, external API ingestion, and internal RPC communication require a standardized HTTP transport and client layer across monitoring and telemetry services.
- Unconfigured HTTP clients default to unbounded timeouts, exposing network-probing daemons to socket exhaustion and hanging routines when target endpoints experience latency or failure.
- Multiple service modules across handlers, background monitoring jobs, and client adapters independently establish outbound network connections and require uniform client lifecycle and timeout management.

## Problem Statement

Outbound network operations across monitoring probes, metric ingestion pipelines, and private location synchronization risk resource starvation and inconsistent lifecycle handling without a unified HTTP client library and mandatory timeout discipline.

## Decision

1. SHOULD: Outbound HTTP requests executed through http.Client.Do or RPC unary call wrappers SHOULD propagate context.Context instances to support cancellation and deadline enforcement.

## Policy Block

- SHOULD Outbound HTTP requests executed through http.Client.Do or RPC unary call wrappers SHOULD propagate context.Context instances to support cancellation and deadline enforcement.

In scope:
- All service handlers, monitoring check jobs, and telemetry client packages executing outbound HTTP requests or hosting RPC endpoints.
- Internal client wrappers managing transport sessions for telemetry ingestion and remote location synchronization.

Out of scope:
- Raw socket protocols that bypass HTTP transport mechanisms entirely.
- In-memory mock transports utilized exclusively within unit testing suites.

## Rationale

- Evidence demonstrates widespread adoption of net/http across service entry points, handlers, external telemetry clients, and RPC stubs, establishing it as the standard networking foundation.
- Configuring explicit Timeout fields on http.Client instances directly observed across checker handlers and background jobs guarantees bounded latency and protects daemons from resource leaks.
- Relying on the core net/http package eliminates third-party transport dependencies while providing native compatibility with Connect RPC protocols and standard context propagation.

## Consequences

Positive:
- Standardizes HTTP client instantiation, request execution, and timeout configuration across all monitoring check handlers and clients.
- Prevents connection hangs, socket exhaustion, and goroutine leaks by enforcing explicit request timeouts across all outbound endpoints.
- Maintains zero external dependency overhead for core HTTP transport while seamlessly integrating with RPC and telemetry layers.

Negative:
- Requires manual configuration of timeouts and transports on every client instantiation instead of relying on global framework defaults.
- Imposes boilerplate code across client wrappers for context propagation, header management, and response body handling.

## Alternatives

- Third-party HTTP client libraries with built-in retry and middleware features (rejected)
  Rejected because: Introduces unnecessary external dependency overhead and divergence from standard Connect RPC transport interfaces when net/http satisfies all functional requirements.
  When valid: Valid when advanced connection pooling, automatic transparent redirect pipelines, or complex middleware graphs are required.
- Unconfigured default HTTP client with ambient system timeouts (rejected)
  Rejected because: Leaves outbound HTTP requests susceptible to indefinite blocking during network degradation, leading to socket exhaustion.
  When valid: Valid only for quick exploratory scripting where process lifecycle terminates immediately.

## Risks

- Individual client instances may omit explicit timeout settings, leading to potential hanging connections under network faults.
  Mitigation: Enforce static analysis linting rules and code review policies that flag client instantiations lacking timeout attributes.
  Owner: Engineering Team
- Response bodies from net/http client requests may fail to be closed properly, causing connection pool starvation.
  Mitigation: Mandate immediate deferral of response body closure following error verification in all client execution paths.
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
- Instantiate http.Client instances with tailored Timeout durations derived from configuration sources or request parameters rather than sharing unconstrained clients.
- Ensure all network request invocations pass an active context to http.NewRequestWithContext or RPC client wrappers to honor process cancellation signals.

## Continuation Context


Verify commands:
- Discover the project test execution script from repository metadata and run the test suite to validate HTTP client timeout enforcement.
- Discover the repository static analysis configuration and execute the linter to verify that no unbounded HTTP client instantiations exist.

Accept when:
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

## Enforcement

- Verified by: Continuous integration automated linting and static analysis checks.
- Verified by: Peer code review verification of all new client instantiations and network handlers.
- Violation handling: Automated pull request check failures blocking merge when unconfigured HTTP clients are detected.
- Violation handling: Mandatory remediation of unbounded client calls before promotion to staging or production environments.
- Exception process: Submission of an architectural review request detailing why an unbounded timeout is strictly required for a specialized streaming connection, signed off by a principal engineer.