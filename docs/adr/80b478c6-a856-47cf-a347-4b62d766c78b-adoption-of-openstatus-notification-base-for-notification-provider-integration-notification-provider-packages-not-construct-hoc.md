---
version: 2
name: 80b478c6-a856-47cf-a347-4b62d766c78b-adoption-of-openstatus-notification-base-for-notification-provider-integration-notification-provider-packages-not-construct-hoc
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Packages Not Construct Hoc"
metadata:
  actual:
    adr_id: 80b478c6-a856-47cf-a347-4b62d766c78b
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Packages Not Construct Hoc

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/telegram`

Applies to:
- `src/**`

## Context

- The codebase contains notification provider packages responsible for dispatching alerts to external communication channels.
- Standardized delivery contracts across multiple notification channels require consistent payload parsing and interface compliance.
- The internal workspace module @openstatus/notification-base provides the canonical foundation and shared abstractions for notification handling.

## Problem Statement

In a multi-channel notification architecture, independent provider packages risk diverging in payload parsing, interface adherence, and secret resolution if each channel implements custom dispatch contracts, leading to maintainability overhead and runtime delivery failures.

## Decision

1. MUST_NOT: Notification provider packages MUST NOT construct ad-hoc internal notification schema definitions that bypass the shared foundational types established by @openstatus/notification-base.

## Policy Block

- MUST_NOT Notification provider packages MUST NOT construct ad-hoc internal notification schema definitions that bypass the shared foundational types established by @openstatus/notification-base.

In scope:
- All notification provider packages responsible for external alert delivery and transport dispatching.
- Integration modules consuming inbound notification payloads within the notification workspace.

Out of scope:
- Core database schema definitions and persistence models managed outside notification delivery modules.
- Independent client applications that trigger notifications without directly executing outbound provider delivery.

## Rationale

- Adopting @openstatus/notification-base establishes a centralized structural contract for all outbound notification integrations across the workspace.
- Enforcing explicit schema parsing on incoming notification payloads prevents malformed data from reaching downstream third-party messaging APIs.
- Separating environment-driven credentials from transport dispatch logic isolates security concerns and ensures channel modularity.

## Consequences

Positive:
- Uniform interface adherence and data validation across all notification delivery providers.
- Reduced boilerplate and eliminated schema drift when adding or modifying notification channels.
- Clear architectural separation between internal notification contracts and external provider HTTP clients.

Negative:
- Provider packages are coupled to changes in the upstream @openstatus/notification-base contract.
- Schema parsing and JSON deserialization introduce marginal processing overhead per notification dispatch.

## Alternatives

- Independent custom schema definitions per notification channel (rejected)
  Rejected because: Causes contract divergence, redundant type declarations, and inconsistent error handling across notification transports.
  When valid: Valid only in standalone single-channel prototypes with no shared infrastructure.
- Direct unvalidated pass-through of raw notification payloads to external APIs (rejected)
  Rejected because: Increases runtime failure rates and leaks provider-specific transport errors without early payload contract validation.
  When valid: Valid only in pass-through reverse proxy systems where payload inspection is intentionally bypassed.

## Risks

- Breaking interface changes in @openstatus/notification-base could disrupt all dependent notification providers simultaneously.
  Mitigation: Enforce contract testing and automated workspace-wide static type verification prior to releasing base module changes.
  Owner: Platform Engineering
- Missing or misconfigured runtime environment secrets can lead to silent or unhandled delivery failures during external API calls.
  Mitigation: Validate the presence of required configuration variables during startup or handler initialization before dispatching requests.
  Owner: Integration Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the workspace package definitions and dependency manifests to identify how internal modules are resolved across packages.
- Verify that incoming notification data strings undergo schema validation immediately upon deserialization before passing structured objects to transport functions.

## Continuation Context


Verify commands:
- Discover and run the project's workspace-wide static type-checking script to confirm full contract adherence with @openstatus/notification-base.
- Discover and execute the test suite corresponding to notification delivery integrations to verify payload parsing and external dispatch behaviors.

Accept when:
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

## Enforcement

- Verified by: Continuous integration type checking and static analysis workflows across all workspace packages.
- Verified by: Peer code review verifying dependency imports and strict schema validation adherence.
- Violation handling: Continuous integration builds fail upon type incompatibility or unvalidated notification payload handling.
- Violation handling: Pull requests bypassing the shared base module contract are blocked until refactored.
- Exception process: Exceptions require formal architectural review and documented rationale demonstrating why the shared base module cannot fulfill transport requirements.