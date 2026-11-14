---
version: 2
name: 5b4e49a6-0c08-4e6f-aff2-d463795345aa-stripe-sdk-adoption-for-subscription-lifecycle-and-billing-management-application-logic-not-bypass-internal-plan
description: "Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Application Logic Not Bypass Internal Plan"
metadata:
  actual:
    adr_id: 5b4e49a6-0c08-4e6f-aff2-d463795345aa
    status: accepted
    candidate_group_id: 34a7a20c-e8d1-4a1c-a1bd-c5720a4ba1b1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/stripe/**
---

# Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Application Logic Not Bypass Internal Plan

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/stripe/**`

## Context

- Subscription-based billing requires resilient coordination between external payment gateways and internal tenant entitlement models.
- Direct or disparate integration with payment provider APIs introduces drift, duplicated cancellation logic, and fragmentation in plan resolution across environments.
- Adopting the Stripe client library centralizes subscription mutations, price resolution, and lifecycle updates within standardized billing router modules.

## Problem Statement

Managing recurring subscription states, cancellation lifecycles, and environment-dependent price mappings without a centralized payment client risks divergent billing logic and inconsistent entitlement enforcement.

## Decision

1. MUST_NOT: Application logic MUST NOT bypass internal plan mapping utilities to hardcode external Stripe price identifiers directly in business components.

## Policy Block

- MUST_NOT Application logic MUST NOT bypass internal plan mapping utilities to hardcode external Stripe price identifiers directly in business components.

In scope:
- All backend modules, services, and route handlers that interact with external billing providers, process subscription updates, or map price identifiers to application plans.

Out of scope:
- Internal entitlement checks and feature flag evaluations that do not directly communicate with external billing infrastructure or resolve external price identifiers.

## Rationale

- Standardizing on the Stripe client library provides strong typing and verified API interactions for subscription management, including scheduled cancellations and metadata recording.
- Mapping external price identifiers to internal schemas through centralized utilities protects core domain entities from direct coupling to payment gateway data shapes.
- Environment-aware price resolution ensures consistent billing behavior across development, preview, and production environments.

## Consequences

Positive:
- Centralized subscription update flows and cancellation handling reduce billing discrepancies and customer entitlement errors.
- Domain models remain cleanly decoupled from external billing schemas through standardized price mapping utilities.
- Maintenance overhead is minimized by relying on supported client library methods for external payment provider calls.

Negative:
- The application architecture becomes tightly coupled to Stripe subscription lifecycle semantics and pricing structures.
- Upgrading the client library requires tracking upstream provider API version changes and breaking changes in subscription data shapes.

## Alternatives

- Custom HTTP client implementation against payment gateway REST endpoints (rejected)
  Rejected because: Requires building and maintaining custom serialization, authentication, error retry handling, and schema validation that the official SDK already provides.
  When valid: When operating in an execution environment with strict bundle constraints where external client dependencies cannot be installed.
- Multi-gateway billing abstraction layer (rejected)
  Rejected because: Adds architectural indirection and maintenance complexity without an immediate requirement to support multiple concurrent payment processors.
  When valid: When business requirements explicitly demand routing payments across multiple merchant aggregators.

## Risks

- Breaking API changes or version deprecations from the external payment provider client library
  Mitigation: Enforce lock-file version verification before implementation and encapsulate external client calls within dedicated router modules.
  Owner: Engineering Team
- Environment configuration mismatch when resolving price identifiers across staging and production
  Mitigation: Validate environment variables at startup and verify price mapping lookups with automated unit tests.
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
- Encapsulate all subscription mutation operations within dedicated billing router modules to prevent client instances from leaking into domain layers.
- Ensure environment-specific price lookup tables are validated against active payment gateway configuration during test execution.

## Continuation Context


Verify commands:
- Discover and execute the project verification test suite for billing and subscription router modules.
- Discover and execute the repository type-checking script across modules interfacing with external payment SDKs.
- Discover and execute the linting and static analysis suite to verify subscription lifecycle integration constraints.

Accept when:
- All subscription lifecycle operations execute through the designated library client methods without direct HTTP bypasses.
- Price lookup utilities accurately resolve environment-specific price identifiers to internal plan definitions.
- Project verification and type-checking suites pass without diagnostic errors.

## Enforcement

- Verified by: Automated type checking and linting executed during continuous integration.
- Verified by: Peer review on all pull requests modifying billing router modules or subscription utilities.
- Verified by: Automated test suites verifying subscription update behaviors and price lookup mappings.
- Violation handling: Continuous integration builds fail upon unapproved direct HTTP calls to payment endpoints or untyped price lookups.
- Violation handling: Pull requests containing unmapped price identifiers or direct SDK instantiation outside billing boundaries will be rejected during review.
- Exception process: Exceptions must be submitted via an architecture review proposal detailing the alternative payment provider or standalone integration requirements.
- Exception process: Requires formal approval from the technical lead and documented security review before implementation.