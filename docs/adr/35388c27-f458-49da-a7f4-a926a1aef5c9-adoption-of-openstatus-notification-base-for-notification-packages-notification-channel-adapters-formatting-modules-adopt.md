---
version: 2
name: 35388c27-f458-49da-a7f4-a926a1aef5c9-adoption-of-openstatus-notification-base-for-notification-packages-notification-channel-adapters-formatting-modules-adopt
description: "Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Adapters Formatting Modules Adopt"
metadata:
  actual:
    adr_id: 35388c27-f458-49da-a7f4-a926a1aef5c9
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Adapters Formatting Modules Adopt

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/ms-teams`

Applies to:
- `src/**`

## Context

- Notification delivery spans multiple external transport adapters that consume notification event context and dispatch formatted messages.
- Independent channel implementations risk diverging in notification payload handling, validation contracts, and base message transformations.
- The codebase demonstrates adoption of the shared internal module @openstatus/notification-base across notification dispatching and card construction logic.

## Problem Statement

Without a common foundational module for notification integrations, individual notification channel adapters define independent context models and data-handling semantics, resulting in schema divergence, maintenance overhead, and inconsistent notification delivery behavior across external messaging providers.

## Decision

1. MUST: Notification channel adapters and formatting modules MUST adopt @openstatus/notification-base as their shared foundational dependency for notification context and contracts.

## Policy Block

- MUST Notification channel adapters and formatting modules MUST adopt @openstatus/notification-base as their shared foundational dependency for notification context and contracts.

In scope:
- All notification integration packages, channel adapters, and message formatting modules responsible for processing notification context.

Out of scope:
- Transport-agnostic utility libraries and database schema definitions that do not participate in notification payload handling.

## Rationale

- Adopting @openstatus/notification-base provides a single source of truth for notification structures, execution contexts, and common notification contracts.
- Shared baseline definitions ensure all notification channel adapters process incoming notification context predictably before transforming payloads for external delivery endpoints.
- Evidence demonstrates that notification formatting and dispatch implementations consistently build upon @openstatus/notification-base rather than creating local domain representations.

## Consequences

Positive:
- Standardizes notification context models, parsing flows, and delivery interfaces across distinct channel adapters.
- Prevents duplication of core notification domain concepts and serialization contracts across integration packages.
- Enables uniform telemetry, validation, and error-handling semantics across all external notification targets.

Negative:
- Changes to shared notification structures in @openstatus/notification-base require coordinated updates and version alignment across all dependent channel packages.
- Direct coupling to a shared internal module introduces a common dependency that must be maintained and published across the workspace.

## Alternatives

- Independent notification channel implementations with local type definitions (rejected)
  Rejected because: Leads to contract divergence, inconsistent notification context structures, and duplicated integration logic across channel adapters.
  When valid: Prototyping a standalone experimental notification channel outside the standard notification pipeline.
- Consolidating all notification channel logic into a single monolithic package (rejected)
  Rejected because: Creates tight coupling between disparate external delivery clients and inflates dependency trees for consumers requiring only specific channels.
  When valid: When the system supports only a single invariant notification destination with no requirement for channel modularity.

## Risks

- Breaking contract changes in @openstatus/notification-base can disrupt multiple notification adapters simultaneously.
  Mitigation: Maintain backward-compatible contract evolution and comprehensive test suites within @openstatus/notification-base before releasing updates.
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
- Discover shared notification payload structures and interfaces from @openstatus/notification-base before implementing channel-specific transformation logic.
- Ensure channel adapters isolate external client transport payloads while mapping from the normalized context supplied by @openstatus/notification-base.

## Continuation Context


Verify commands:
- Discover and execute the repository dependency boundary validation check to verify that notification channel modules consume @openstatus/notification-base.
- Discover and execute the repository build and type verification suite to ensure contract alignment with @openstatus/notification-base.

Accept when:
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

## Enforcement

- Verified by: Automated dependency analysis during continuous integration confirming consumption of @openstatus/notification-base in notification packages.
- Verified by: Peer review during code changes affecting notification integrations and shared notification interfaces.
- Violation handling: Pull requests introducing ad-hoc notification structures without @openstatus/notification-base MUST be blocked at continuous integration.
- Violation handling: Flagged violations MUST refactor integration code to adhere to shared notification contracts prior to merge.
- Exception process: Submit an architectural exception request detailing why a specialized notification integration cannot consume @openstatus/notification-base.
- Exception process: Obtain sign-off from notification infrastructure maintainers prior to merging uncoupled delivery logic.