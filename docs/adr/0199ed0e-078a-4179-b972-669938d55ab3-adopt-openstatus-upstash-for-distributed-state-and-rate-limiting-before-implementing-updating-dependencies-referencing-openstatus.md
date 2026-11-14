---
version: 2
name: 0199ed0e-078a-4179-b972-669938d55ab3-adopt-openstatus-upstash-for-distributed-state-and-rate-limiting-before-implementing-updating-dependencies-referencing-openstatus
description: "Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Before Implementing Updating Dependencies Referencing Openstatus"
metadata:
  actual:
    adr_id: 0199ed0e-078a-4179-b972-669938d55ab3
    status: accepted
    candidate_group_id: 50df5cbd-07a3-4e48-808a-e3b273ebd893
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
      - src/service/**
---

# Adopt `@openstatus/upstash` for Distributed State and Rate Limiting: Before Implementing Updating Dependencies Referencing Openstatus

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`
- `src/service/**`

## Context

- API route handlers and background update processes require distributed caching, key-value storage, and request rate limiting across multiple operational endpoints.
- Direct instantiation of remote datastore clients within individual handlers leads to configuration drift, duplicate connection initialization, and inconsistent error handling.
- Standardizing on the shared internal module establishes a unified interface for distributed key-value operations and request rate limiting throughout the service layer.

## Problem Statement

Without a unified module for distributed state and rate limiting, independent API routes and background services risk fragmented connection management, disparate error handling semantics, and uncoordinated rate limits across distributed instances. Establishing a centralized workspace module provides predictable access, standardized client configuration, and resilient fallback degradation during network anomalies.

## Decision

1. MUST: Before implementing or updating dependencies referencing `@openstatus/upstash` or its underlying drivers, engineers MUST discover the repository dependency manifest and lock artifact to resolve the exact locked version.

## Policy Block

- MUST Before implementing or updating dependencies referencing `@openstatus/upstash` or its underlying drivers, engineers MUST discover the repository dependency manifest and lock artifact to resolve the exact locked version.

In scope:
- All public and authenticated API route procedures requiring request rate limiting or distributed key-value operations.
- Background service workers requiring distributed state synchronization or shared counter tracking.

Out of scope:
- Pure computational routines that execute without cross-process or cross-request state dependencies.
- Client-side rendering and static asset presentation layers.

Exceptions:
- EXC-20-001: Local testing environments operate without access to distributed datastore infrastructure and require in-memory mock clients.

## Rationale

- Centralizing distributed key-value and rate-limiting operations in `@openstatus/upstash` prevents duplication of client initialization logic across route handlers and background workers.
- Encapsulating datastore access within a shared workspace module enables uniform graceful degradation, ensuring that datastore hiccups do not trigger uncaught service failures.
- Isolates upstream driver configurations from consumer endpoint contracts, simplifying maintenance and credential lifecycle governance.

## Consequences

Positive:
- Enforces consistent rate-limiting logic and distributed caching behavior across public endpoints and background services.
- Standardizes resilient fallback policies to prevent remote datastore disruptions from producing public-facing outages.
- Centralizes configuration and credential management for distributed datastore operations.

Negative:
- Couples route handlers and service modules to the internal shared workspace package interface.
- Incurs remote network latency during distributed rate-limit checks and state synchronization.

## Alternatives

- Direct third-party datastore SDK instantiation in individual router and service files (rejected)
  Rejected because: Introduces duplicated connection overhead, inconsistent rate-limiting thresholds, and fragmented error handling across endpoints.
  When valid: Valid only in single-file prototypes or standalone scripts with no shared state requirements.
- Process-local in-memory state tracking and rate limiting (rejected)
  Rejected because: In-memory storage cannot synchronize state across distributed serverless instances or separate background service workers.
  When valid: Valid for single-instance development environments or ephemeral test fixtures.

## Risks

- Remote network latency or datastore outages impacting request handling performance.
  Mitigation: Implement non-blocking fallbacks and exception handling around rate-limiting calls so operations proceed when datastore operations fail.
  Owner: Engineering team
- Upstream driver API changes breaking consumer routes across multiple packages.
  Mitigation: Encapsulate third-party client interactions within the shared module interface and enforce strict lock-file version verification.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Wrap all rate-limiting and distributed cache invocations in defensive error-handling logic that logs diagnostic warnings and enables graceful fallback.
- Derive rate-limiting client keys from request context headers rather than relying on ephemeral local socket addresses.

## Continuation Context


Verify commands:
- Discover and execute the project workspace test suite to verify module resolution and integration for the shared datastore module.
- Discover and run the project code linter and type checker to confirm import compliance across all routing and service modules.

Accept when:
- All automated test suites pass with zero module resolution errors for the shared datastore dependency.
- Static analysis and import graph validation confirm no unauthorized direct datastore driver imports outside the shared workspace module.

## Enforcement

- Verified by: Automated import boundary validation and static analysis checks during integration pipelines.
- Verified by: Integration tests verifying non-blocking fallback behavior when distributed datastore operations encounter simulated failures.
- Violation handling: Pull requests introducing direct external datastore client instantiations outside the designated workspace module will be blocked during code review.
- Exception process: Submit an architectural review request outlining specific isolation or performance requirements that cannot be satisfied by the shared module.