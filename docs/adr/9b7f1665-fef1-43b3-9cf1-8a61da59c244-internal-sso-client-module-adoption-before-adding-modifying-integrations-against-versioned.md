---
version: 2
name: 9b7f1665-fef1-43b3-9cf1-8a61da59c244-internal-sso-client-module-adoption-before-adding-modifying-integrations-against-versioned
description: "Internal SSO Client Module Adoption: Before Adding Modifying Integrations Against Versioned"
metadata:
  actual:
    adr_id: 9b7f1665-fef1-43b3-9cf1-8a61da59c244
    status: accepted
    candidate_group_id: 4b04e903-9c47-4b5d-ac86-aac2aaa2bd09
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/sso/**
---

# Internal SSO Client Module Adoption: Before Adding Modifying Integrations Against Versioned

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/sso/**`

## Context

- Service handlers managing identity federation, portal session creation, and webhook events require consistent communication with underlying single sign-on providers.
- Bypassing centralized client abstractions creates fragmented authentication logic, inconsistent error propagation, and unmanaged credential distribution across distinct endpoint handlers.
- Static analysis of single sign-on service endpoints demonstrates uniform dependence on a shared internal client module paired with common domain context and error structures.

## Problem Statement

Direct, uncoordinated communication with single sign-on infrastructure across heterogeneous service entry points leads to inconsistent authentication payloads, fragmented protocol handling, and duplicated client lifecycle management.

## Decision

1. MUST: Before adding or modifying integrations against versioned external client libraries that underpin the internal client module, consumers MUST verify resolved versions against the project lock resolution artifact in accordance with the lock-version grounding policy.

## Policy Block

- MUST Before adding or modifying integrations against versioned external client libraries that underpin the internal client module, consumers MUST verify resolved versions against the project lock resolution artifact in accordance with the lock-version grounding policy.

In scope:
- Service handlers and endpoints implementing single sign-on flows, directory synchronizations, portal link generations, and webhook event processing.

Out of scope:
- Utility functions and standalone validation routines that do not communicate with external or upstream identity provider infrastructure.

## Rationale

- Centralizing identity provider communications inside a dedicated client module enforces uniform security controls, authentication headers, and request formatting across disparate handlers.
- Consistent error handling and context propagation across portal linking, retrieval, and webhook operations prevent unhandled provider faults from destabilizing calling services.
- Encapsulation of provider interfaces simplifies future identity infrastructure migrations and protocol version upgrades by localizing external protocol changes to a single module boundary.

## Consequences

Positive:
- Establishes a single point of maintenance for external identity provider integration logic, authentication credentials, and request transport.
- Standardizes error propagation and execution context tracing across all identity federation workflows.
- Decouples handler layers so they focus purely on business input validation and workflow orchestration.

Negative:
- Introduces an additional layer of module indirection for simple single-field retrieval or forwarding operations.
- New identity provider features require client module interface extensions before service handlers can consume them.

## Alternatives

- Direct inline transport client instantiation within each individual service handler (rejected)
  Rejected because: Scatters identity provider authentication, network timeout configuration, and protocol handling across disparate endpoint files, resulting in behavioral divergence.
  When valid: Valid only in single-file prototypes or exploratory proofs of concept with no production service requirements.
- Global ambient identity provider client singleton instantiated at application startup (rejected)
  Rejected because: Prevents request-scoped context propagation, complicates multi-tenant configuration, and hinders unit testing isolation.
  When valid: Valid only in single-tenant applications with static, process-wide authentication parameters.

## Risks

- The internal client module could become a monolithic bottleneck if divergent provider interfaces are merged without clear domain boundaries.
  Mitigation: Decompose complex provider subsystems into focused internal submodules coordinated by the primary client interface.
  Owner: engineering team
- Upstream API breaking changes in underlying identity providers could break all consuming handlers simultaneously.
  Mitigation: Maintain contract validation and automated regression tests around the client module boundary prior to deploying dependency updates.
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
- Ensure that internal client module methods accept standardized request context parameters to facilitate distributed tracing and tenancy resolution.
- Isolate provider-specific payload serialization and deserialization routines within internal client helpers rather than exposing raw provider data structures to service entry points.

## Continuation Context


Verify commands:
- Discover the project static analysis and linting task configurations to verify that service handlers import identity client capabilities strictly from the authorized internal client module.
- Discover and execute repository test runners across the service test suite to validate that identity workflows execute with expected client mocking and context propagation.

Accept when:
- Static import boundary checks pass with zero unauthorized direct transport or external identity client imports across service handlers.
- All unit and integration test suites covering single sign-on workflows execute successfully with complete coverage of client module interaction paths.

## Enforcement

- Verified by: Automated static analysis checks enforcing module import boundaries during continuous integration.
- Verified by: Peer code review verification confirming all identity provider interactions traverse the internal client module.
- Violation handling: Pull requests containing direct external client instantiations or unapproved transport calls within service handlers are blocked from merging.
- Violation handling: Non-compliant imports flagged during review require refactoring to utilize the internal client interface prior to approval.
- Exception process: Exceptions for specialized integration paths must be submitted via an architectural review request and approved by the domain technical leads.