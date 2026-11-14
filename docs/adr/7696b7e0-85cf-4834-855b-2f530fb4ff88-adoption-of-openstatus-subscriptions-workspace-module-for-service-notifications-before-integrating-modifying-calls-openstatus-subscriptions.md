---
version: 2
name: 7696b7e0-85cf-4834-855b-2f530fb4ff88-adoption-of-openstatus-subscriptions-workspace-module-for-service-notifications-before-integrating-modifying-calls-openstatus-subscriptions
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Before Integrating Modifying Calls Openstatus Subscriptions"
metadata:
  actual:
    adr_id: 7696b7e0-85cf-4834-855b-2f530fb4ff88
    status: accepted
    candidate_group_id: 661dd269-6c9f-4493-9f3c-300f559dd7a2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/maintenance/**
      - src/page-subscriber/**
      - src/status-report/**
---

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Before Integrating Modifying Calls Openstatus Subscriptions

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/maintenance/**`
- `src/page-subscriber/**`
- `src/status-report/**`

## Context

- Service workflows trigger notifications across multiple operational contexts, including maintenance announcements, scheduled status reports, and subscriber webhook verification.
- Independent notification mechanisms within individual service handlers create risks of divergent subscriber delivery rules, inconsistent retry semantics, and duplicated channel configuration logic.
- The service layer consolidates subscriber notification processing by importing the shared @openstatus/subscriptions internal workspace module alongside shared database schemas, authentication, and execution context.
- Incoming service request arguments undergo structured input validation parsing prior to invoking notification delivery logic, ensuring clean payload boundaries between the caller and the subscriber subsystem.

## Problem Statement

Services that notify subscribers or trigger webhook dispatches require a uniform mechanism for subscriber resolution, channel dispatch, and payload delivery. Without a centralized domain module governing subscriptions, individual service routines would independently implement channel iteration, credential retrieval, and webhook execution, leading to maintenance overhead, inconsistent delivery states, and fragmented channel error handling.

## Decision

1. MUST: Before integrating or modifying calls to @openstatus/subscriptions, the consumer MUST discover the workspace dependency manifest and inspect the repository resolution lock artifact to establish the exact resolved workspace package version.

## Policy Block

- MUST Before integrating or modifying calls to @openstatus/subscriptions, the consumer MUST discover the workspace dependency manifest and inspect the repository resolution lock artifact to establish the exact resolved workspace package version.

In scope:
- Service-layer handlers and endpoints that broadcast notifications, execute test webhooks, or process subscriber communication events.
- Notification workflows requiring coordination with subscriber records and channel configurations.

Out of scope:
- Internal service routines that do not interact with subscriber notification channels or delivery targets.
- Pure data ingestion or monitoring collection routines with no subscriber dispatch requirements.

## Rationale

- Centralizing notification dispatch within @openstatus/subscriptions decouples service business logic from channel-specific delivery and subscriber state tracking.
- Coupling subscriber notification handlers with shared database schemas and authentication context provides consistent access control and auditability across all alert types.
- Evidence across multiple notification entrypoints confirms that validating inputs prior to dispatch prevents malformed payloads from entering subscriber delivery pipelines.

## Consequences

Positive:
- Ensures unified delivery policies and consistent channel formatting across maintenance alerts, status reports, and subscriber test webhooks.
- Eliminates duplicate subscriber iteration and webhook delivery logic across distinct service endpoints.
- Simplifies service testability by isolating subscriber dispatch logic behind a dedicated domain module interface.

Negative:
- Introduces an internal workspace dependency coupling between notification services and the subscription module.
- Modifications to subscriber delivery contracts require synchronized updates across all dependent service handlers.

## Alternatives

- Direct inline webhook dispatch and channel notification logic inside each service handler (rejected)
  Rejected because: Duplicating channel formatting, retry loops, and webhook dispatch across service handlers introduces maintenance divergence and increases defect surface area.
  When valid: Prototyping standalone isolated endpoints that require no subscriber coordination or persistence.
- External message broker topic publishing for asynchronous decoupling (deferred)
  Rejected because: Direct invocation of the shared subscription domain module satisfies current throughput and transactional consistency requirements without operational queue infrastructure.
  When valid: When notification volume exceeds synchronous processing limits and requires distributed background workers.

## Risks

- Breaking contract changes in @openstatus/subscriptions impacting multiple notification services simultaneously.
  Mitigation: Enforce workspace-wide static type checking and comprehensive integration tests across all dependent service packages before publishing module updates.
  Owner: Service Engineering Team
- Unhandled delivery failures in external subscriber channels blocking service execution.
  Mitigation: Ensure subscriber dispatch operations handle channel timeouts gracefully and record failure states without crashing caller transactions.
  Owner: Service Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all service notification handlers parse and sanitize input arguments through their declared schema validation routines before invoking @openstatus/subscriptions functions.
- Pass the shared context and authentication objects into subscription handlers to preserve tenancy, audit trails, and execution context throughout notification dispatch.

## Continuation Context


Verify commands:
- Discover and execute the project static type validation script to verify that all service notification entrypoints import and consume @openstatus/subscriptions without type errors.
- Discover and execute the workspace test suite covering service notification routines to confirm successful subscriber resolution and webhook dispatch behavior.

Accept when:
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

## Enforcement

- Verified by: Continuous integration automated static type verification and test suite execution
- Verified by: Mandatory peer code review for any changes to service notification handlers and subscription modules
- Violation handling: Pull requests introducing direct channel dispatch or bypassing @openstatus/subscriptions will be blocked at continuous integration and code review.
- Exception process: Submit an architectural review request outlining why the shared subscription module cannot support the required notification pattern, along with proposed module extensions.