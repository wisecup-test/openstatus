---
version: 2
name: 5bd20069-922d-4b7b-96d3-ee128bf2ad69-adoption-of-openstatus-notification-base-for-subscription-notification-channels-before-implementing-code-depending-versioned-modules
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Before Implementing Code Depending Versioned Modules"
metadata:
  actual:
    adr_id: 5bd20069-922d-4b7b-96d3-ee128bf2ad69
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Before Implementing Code Depending Versioned Modules

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/channels/**`

## Context

- The subscription delivery system must support heterogeneous dispatch mechanisms while maintaining consistent notification semantics.
- Multiple channel implementations independently manage payload dispatch and integration boundaries.
- Adopting a shared internal module standardizes domain structures across all channel delivery mechanisms.

## Problem Statement

Subscription delivery mechanisms across channels risk divergence in payload structures, notification lifecycle management, and contract enforcement when implemented independently without a common foundational library.

## Decision

1. MUST: Before implementing code depending on versioned modules, developers MUST locate the project resolution artifact, inspect the authoritative resolved dependency version, and verify API compatibility against the exact version documentation.

## Policy Block

- MUST Before implementing code depending on versioned modules, developers MUST locate the project resolution artifact, inspect the authoritative resolved dependency version, and verify API compatibility against the exact version documentation.

In scope:
- All subscription notification channels and message dispatch modules within the system.
- Modules responsible for structuring or sending event notifications to external destinations.

Out of scope:
- Standalone protocol clients and utility modules unrelated to notification dispatch.
- External integration adapters that operate completely outside internal subscription notification workflows.

## Rationale

- Evidence indicates consistent adoption of @openstatus/notification-base across distinct notification channels to enforce common domain schemas.
- Centralizing core notification contracts inside a dedicated shared module ensures architectural consistency across disparate dispatch mechanisms.

## Consequences

Positive:
- Guarantees consistent notification data modeling and interface contracts across diverse dispatch channels.
- Reduces code duplication across channel implementations by centralizing common notification logic.
- Simplifies onboarding of new notification channels by providing established baseline contracts.

Negative:
- Couples multiple delivery channels to the release lifecycle of the shared internal module.
- Changes to core notification contracts require coordinated regression testing across all channel implementations.

## Alternatives

- Decentralized channel-specific payload and contract definitions (rejected)
  Rejected because: Defining independent schemas per channel causes contract drift and forces duplicate maintenance across dispatch handlers.
  When valid: Valid only when notification channels share zero domain properties or dispatch lifecycle stages.
- Direct third-party client invocation without shared internal module abstractions (rejected)
  Rejected because: Couples notification dispatching directly to external service structures without a standard internal notification boundary.
  When valid: Valid only in isolated one-off utility scripts or throwaway integration spikes.

## Risks

- Breaking changes introduced to the shared base library can simultaneously disrupt all subscribing channel dispatchers.
  Mitigation: Enforce strict version pinning in resolution artifacts and comprehensive test suites across all channel consumers.
  Owner: engineering team
- Channel-specific concerns may leak into the shared library, polluting the common abstraction with transport-specific logic.
  Mitigation: Restrict the shared module strictly to channel-agnostic domain contracts and maintain transport details in individual channel packages.
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
- Locate the repository dependency manifest and resolution artifacts to discover the exact resolved version of @openstatus/notification-base.
- Maintain clean architectural separation by keeping transport-specific networking and protocol handling inside the channel module while delegating core data modeling to the shared library.

## Continuation Context


Verify commands:
- grep -rn "@openstatus/notification-base" .
- find . -type f -exec grep -l "@openstatus/notification-base" {} +

Accept when:
- All notification channel modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.
- Automated project verification and type checking scripts execute successfully with zero dependency resolution errors.

## Enforcement

- Verified by: Static analysis and dependency scanning during continuous integration checks.
- Verified by: Peer review verifying that channel implementations derive payloads from the shared module.
- Violation handling: Pull requests introducing custom notification domain models without consuming the shared module will be blocked.
- Violation handling: Flagged violations require refactoring channel code to align with the core notification abstraction.
- Exception process: Submit an architecture review request detailing the technical limitations that prevent using the shared core module.
- Exception process: Obtain sign-off from the lead maintainer before committing channel implementations that bypass the shared library.