---
version: 2
name: 9581560d-2d69-413f-a499-b65e0a2da72e-adoption-of-openstatus-notification-base-for-notification-channel-modules-channel-specific-message-block-builders-webhook
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Channel Specific Message Block Builders Webhook"
metadata:
  actual:
    adr_id: 9581560d-2d69-413f-a499-b65e0a2da72e
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Channel Specific Message Block Builders Webhook

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/slack`

Applies to:
- `src/**`

## Context

- The notification subsystem coordinates message delivery across external communication services via specialized provider packages.
- Different notification channels require consistent core interfaces, payload contracts, and shared domain models to ensure reliable delivery and consistent alerting behavior.
- Multiple modules within notification channel packages import and rely upon @openstatus/notification-base to provide standardized baseline notification capabilities.

## Problem Statement

Implementing multiple external notification channels without a standardized foundation leads to divergent notification schemas, duplicated serialization logic, and inconsistent message dispatch semantics across integration packages.

## Decision

1. SHOULD: Channel-specific message block builders and webhook dispatchers SHOULD consume base notification types from @openstatus/notification-base while encapsulating vendor-specific payload formatting within their respective packages.

## Policy Block

- SHOULD Channel-specific message block builders and webhook dispatchers SHOULD consume base notification types from @openstatus/notification-base while encapsulating vendor-specific payload formatting within their respective packages.

In scope:
- All notification integration packages, channel providers, message block formatters, and dispatch handlers within the notification subsystem.

Out of scope:
- Generic database schema definition packages, non-notification utility modules, and standalone services that do not format or dispatch notification messages.

## Rationale

- Static analysis reveals consistent imports of @openstatus/notification-base across multiple modules within notification integration packages, substantiating deliberate reliance on a shared internal domain foundation.
- Centralizing baseline notification models and contracts in @openstatus/notification-base ensures cross-channel consistency and simplifies the onboarding of new notification integrations.
- Maintaining distinct notification provider packages that depend on a shared core library preserves modularity while avoiding monolithic coupling across external vendor SDKs.

## Consequences

Positive:
- Guarantees consistent notification data structures and lifecycle contracts across distinct notification delivery providers.
- Reduces boilerplate and eliminates redundant data transformations across individual notification channel adapters.
- Facilitates testing and maintainability by isolating core notification primitives into a reusable module.

Negative:
- Changes to shared contracts in @openstatus/notification-base require coordinated testing across all dependent notification channel modules.
- Couples individual notification packages to the release lifecycle of the shared internal base module.

## Alternatives

- Directly authoring independent notification payload and channel dispatch logic in each notification package without a shared base library (rejected)
  Rejected because: Creates code duplication across channel packages and causes contract divergence for notification payloads.
  When valid: Valid only when a notification transport requires completely bespoke protocols incompatible with unified notification primitives.
- Consolidating all notification channel adapters into a single monolithic package rather than shared base library and individual channel modules (rejected)
  Rejected because: Forces unnecessary coupling and bundles irrelevant third-party dependencies for environments that only require specific notification channels.
  When valid: Valid only for simple single-channel applications with no planned expansion to other notification destinations.

## Risks

- Breaking changes introduced in @openstatus/notification-base risk cascading compilation and runtime failures across all notification channel packages.
  Mitigation: Maintain strict semver compatibility and run integration test suites across all downstream channel packages upon modifying shared base exports.
  Owner: Notification Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inspect the workspace package dependency declarations to incorporate @openstatus/notification-base into any newly introduced notification provider packages.
- Construct channel-specific payload builders and webhook dispatch logic around the shared baseline abstractions exported by @openstatus/notification-base.

## Continuation Context


Verify commands:
- Discover the workspace dependency graph and verify that notification channel packages declare and resolve a dependency on @openstatus/notification-base.
- Discover and execute the repository static analysis and typecheck verification routines to validate that notification channel modules compile cleanly against @openstatus/notification-base.

Accept when:
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

## Enforcement

- Verified by: Automated workspace dependency boundary checks and static analysis in continuous integration pipelines.
- Verified by: Mandatory peer code review for all modifications in notification packages.
- Violation handling: Pull requests introducing uncoordinated notification payload structures or bypassing @openstatus/notification-base are blocked at continuous integration.
- Violation handling: Code review rejections require aligning custom notification logic with the shared base library interfaces.
- Exception process: Teams seeking an exception must submit an architectural variance detailing why target notification primitives cannot be represented via @openstatus/notification-base.
- Exception process: Exceptions require formal sign-off from the architecture review team prior to merging.