---
version: 2
name: c84f6b5a-c579-4734-b4d3-fe1467be3404-adoption-of-openstatus-notification-base-for-notification-providers-notification-dispatch-error-handling-log-failure
description: "Adoption of @openstatus/notification-base for Notification Providers: Notification Dispatch Error Handling Log Failure"
metadata:
  actual:
    adr_id: c84f6b5a-c579-4734-b4d3-fe1467be3404
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: Notification Dispatch Error Handling Log Failure

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/google-chat`

Applies to:
- `src/**`

## Context

- The notification subsystem communicates with multiple third-party services to deliver system alerts and status updates.
- Individual notification packages require standard interfaces for receiving, validating, and formatting event payloads.
- The internal module @openstatus/notification-base provides shared abstractions and contracts for notification channel implementations.
- Evidence demonstrates the adoption of @openstatus/notification-base alongside schema parsing and external client request boundaries.

## Problem Statement

Implementing notification channel providers without a shared foundational module creates diverging data contracts, redundant transformation routines, and inconsistent error handling across notification integrations. A shared core module is required to enforce uniform contracts across notification packages.

## Decision

1. SHOULD: Notification dispatch error handling SHOULD log failure details while preventing leakage of sensitive credentials or endpoints.

## Policy Block

- SHOULD Notification dispatch error handling SHOULD log failure details while preventing leakage of sensitive credentials or endpoints.

In scope:
- Packages and modules implementing notification delivery channels and alert dispatch.
- Code responsible for validating and transforming incoming notification event payloads.

Out of scope:
- Standalone utility packages unrelated to notification dispatch.
- Database migration scripts and persistence schema definitions.

## Rationale

- Adopting @openstatus/notification-base guarantees consistent notification contract structures across provider integrations.
- Extracting shared notification interfaces into an internal package minimizes code duplication across alert channels.
- Decoupling base notification contracts from concrete transport logic ensures maintainability across provider implementations.

## Consequences

Positive:
- Enforces uniform contract structures and shared behavior across all notification channel implementations.
- Centralizes common notification handling logic, reducing maintenance burden and code duplication.
- Establishes clear architectural boundaries between payload parsing and external network dispatch.

Negative:
- Couples notification channel packages to the interface lifecycle and breaking changes of the base module.
- Requires coordinated workspace package version updates across dependent provider modules.

## Alternatives

- Independent implementation of notification structures and contracts within each provider package (rejected)
  Rejected because: Results in redundant logic, fragmented schema definitions, and inconsistent error handling across notification channels.
  When valid: Valid only in single-channel deployments with no planned future notification integrations.
- Consolidating notification contracts into generic database or utility modules (rejected)
  Rejected because: Couples general utility and persistence layers to specific external dispatch semantics, violating domain separation.
  When valid: Valid in monolithic systems where notification dispatch is directly coupled to database triggers.

## Risks

- Breaking interface changes in the base module cascading across all notification channel packages.
  Mitigation: Maintain stable interface contracts in the base module and run cross-package integration test suites before releases.
  Owner: engineering team
- Over-constraining channel-specific notification capabilities through rigid shared abstractions.
  Mitigation: Design base module abstractions to be extensible for provider-specific payload requirements.
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
- Notification provider implementations should import foundational interfaces and common types from @openstatus/notification-base while encapsulating provider-specific payload transformations locally.
- Incoming notification data must undergo schema validation immediately upon reception before dispatching requests across external network boundaries.

## Continuation Context


Verify commands:
- Discover and run the repository typecheck script to verify type compatibility with the base notification module across packages.
- Discover and run the workspace test runner to validate notification payload handling and dispatch flows.
- Discover and execute the repository linter script to ensure compliance with module import and boundary rules.

Accept when:
- All notification provider implementations compile cleanly against the shared base module interfaces and pass static type verification.
- Automated unit and integration test suites validating notification payload parsing and dispatch execute with zero failures.
- Payload validation schemas successfully parse valid notification inputs and reject malformed structures prior to dispatch.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type verification and test suites on pull requests.
- Verified by: Peer code review confirming adherence to base module contracts and separation of dispatch boundaries.
- Violation handling: Pull requests introducing duplicate notification contracts or bypassing the base module are blocked until updated.
- Violation handling: Ad-hoc notification structures must be refactored into the shared base module before merge.
- Exception process: Any proposal to bypass the shared notification base module requires architecture review and documented justification.
- Exception process: Approved exceptions must be tracked with a planned migration timeline to reconcile contracts.