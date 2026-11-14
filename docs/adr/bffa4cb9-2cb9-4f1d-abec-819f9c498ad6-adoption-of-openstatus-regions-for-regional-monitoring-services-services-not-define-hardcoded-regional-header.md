---
version: 2
name: bffa4cb9-2cb9-4f1d-abec-819f9c498ad6-adoption-of-openstatus-regions-for-regional-monitoring-services-services-not-define-hardcoded-regional-header
description: "Adoption of @openstatus/regions for Regional Monitoring Services: Services Not Define Hardcoded Regional Header"
metadata:
  actual:
    adr_id: bffa4cb9-2cb9-4f1d-abec-819f9c498ad6
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: Services Not Define Hardcoded Regional Header

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/monitor/**`

## Context

- Monitoring services coordinate distributed execution across multiple geographic network regions to verify endpoint availability and response dynamics.
- Disparate service handlers previously risked diverging on regional identifiers, routing headers, and regional timeout limits.
- The codebase establishes an internal workspace library @openstatus/regions to provide an authoritative contract for regional metadata, routing headers, and timeout configurations.

## Problem Statement

Monitoring checks and preview streams must coordinate with geographically distributed execution nodes using uniform regional configurations, headers, and execution constraints. Without an authoritative regional abstraction, individual service routines risk defining inconsistent regional identifiers, custom routing headers, and arbitrary timeout windows, leading to fragmented telemetry and unreliable probe dispatch.

## Decision

1. MUST_NOT: Services MUST NOT define hardcoded regional header strings or duplicate regional enum identifiers outside of @openstatus/regions.

## Policy Block

- MUST_NOT Services MUST NOT define hardcoded regional header strings or duplicate regional enum identifiers outside of @openstatus/regions.

In scope:
- All monitoring domain service handlers and preview streaming routines that interface with regional probe endpoints.
- Internal database queries and service workflows requiring geographic region metadata.

Out of scope:
- Local administrative scripts that do not interface with regional probe networks.
- Non-network utility routines operating independently of geographic region context.

## Rationale

- Adopting @openstatus/regions as a shared core module guarantees consistent regional contract definitions across both internal database querying routines and streaming preview execution handlers.
- Centralizing regional header generation and timeout boundaries mitigates configuration drift when regional probe infrastructure evolves.
- A unified regional contract enables predictable telemetry aggregation across geographically dispersed endpoints.

## Consequences

Positive:
- Standardizes regional routing headers, identifiers, and execution timeouts across all monitoring service operations.
- Prevents duplicate or inconsistent geographic definitions across internal queries and preview streaming handlers.
- Simplifies global probe topology updates by centralizing regional configurations in a single module.

Negative:
- Establishes a shared workspace dependency on @openstatus/regions, requiring synchronized updates across all consuming service modules.
- Couples external client dispatch routines to the specific header and timeout contracts exposed by the regional module.

## Alternatives

- Decentralized regional configuration defined locally within individual service modules (rejected)
  Rejected because: Introduced significant risk of configuration drift regarding regional keys, header names, and timeout durations across services.
  When valid: Acceptable only in single-region architectures without distributed probing requirements.
- Direct environment variable resolution for regional endpoints without a shared library interface (rejected)
  Rejected because: Lacked compile-time contract enforcement and centralized typing for regional routing headers across service boundaries.
  When valid: Viable for minimal standalone integration tests targeting a single static gateway.

## Risks

- Breaking contract changes in @openstatus/regions exports can disrupt multiple monitoring service routines simultaneously.
  Mitigation: Enforce strict workspace contract validation and comprehensive regression test suites across consuming services prior to releasing regional updates.
  Owner: engineering team
- Incomplete timeout signal propagation during regional dispatches could lead to unmanaged connection retention.
  Mitigation: Require explicit timeout signal parameters derived from regional constants in all external regional client calls.
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
- Verify that all outgoing HTTP client dispatches targeting regional endpoints spread standardized regional headers derived from @openstatus/regions.
- Configure regional dispatch client calls with abort signal timeouts calibrated to regional timeout constants exported by @openstatus/regions.

## Continuation Context


Verify commands:
- Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services.
- Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling.

Accept when:
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

## Enforcement

- Verified by: Automated static analysis and type checking in continuous integration pipelines.
- Verified by: Peer review verification ensuring all regional routing headers and configurations originate from the shared library.
- Violation handling: Continuous integration build failure upon detection of ad-hoc regional header definitions or duplicate regional enums.
- Violation handling: Pull request blocking until regional interactions conform to the shared library contract.
- Exception process: Submit an architectural review proposal detailing why a specialized regional interaction cannot be fulfilled through @openstatus/regions contracts.