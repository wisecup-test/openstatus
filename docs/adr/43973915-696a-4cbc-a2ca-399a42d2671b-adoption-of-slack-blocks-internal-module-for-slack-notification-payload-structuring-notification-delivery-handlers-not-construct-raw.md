---
version: 2
name: 43973915-696a-4cbc-a2ca-399a42d2671b-adoption-of-slack-blocks-internal-module-for-slack-notification-payload-structuring-notification-delivery-handlers-not-construct-raw
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Notification Delivery Handlers Not Construct Raw"
metadata:
  actual:
    adr_id: 43973915-696a-4cbc-a2ca-399a42d2671b
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Notification Delivery Handlers Not Construct Raw

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/channels/**`

## Context

- Notification delivery channels require structured message formatting and stateful anchor blocks to coordinate real-time updates and subscriber status reports.
- Direct inline definition of Slack Block Kit structures inside channel transport routines couples message presentation logic with subscriber routing and store persistence.
- A shared internal module slack-blocks coordinates block definitions and layout contracts across both channel notification delivery and message store caching layers.

## Problem Statement

Scattering Slack message layout structures, update payload shapes, and block anchor definitions across transport delivery handlers and storage layers introduces inconsistent payload schemas, complicates anchor cache invalidation, and violates separation of concerns between channel dispatch and block presentation.

## Decision

1. MUST_NOT: Notification delivery handlers MUST NOT construct raw Slack Block Kit JSON structures or bypass the slack-blocks module when preparing channel updates.

## Policy Block

- MUST_NOT Notification delivery handlers MUST NOT construct raw Slack Block Kit JSON structures or bypass the slack-blocks module when preparing channel updates.

In scope:
- Constructing Slack message payloads and block layouts for subscription notifications
- Persisting and resolving Slack message anchor structures in channel cache layers

Out of scope:
- Non-Slack notification channel payloads such as email or webhook deliveries
- Direct database schema updates and subscriber state management outside message block formatting

## Rationale

- Centralizing Slack Block Kit schema generation within the slack-blocks module guarantees unified presentation logic and eliminates duplicate JSON block composition across the subscription subsystem.
- Sharing anchor block representations between delivery handlers and persistence mechanisms ensures that cached message references remain synchronized with delivered channel updates.
- Decoupling message block definitions from transport clients allows independent testing and validation of message layouts without mocking network boundaries.

## Consequences

Positive:
- Provides a single source of truth for all Slack notification layouts and anchor metadata across channel subsystems.
- Improves testability of Slack message structures independently of external delivery APIs and cache storage.
- Eliminates duplicate block schema construction across notification handlers and storage coordinators.

Negative:
- Requires ongoing maintenance of an internal domain module to support evolving Slack Block Kit specifications.
- Introduces a shared dependency between the delivery mechanism and channel storage modules.

## Alternatives

- Inline raw Block Kit payload construction directly within channel dispatch functions (rejected)
  Rejected because: Duplicating Block Kit JSON structures inside transport handlers couples delivery mechanics with presentation schemas and prevents reuse in anchor storage tracking.
  When valid: Valid only in single-file prototypes where message schemas are never shared, persisted, or updated.
- Adopting third-party builder libraries for Block Kit assembly across all channels (rejected)
  Rejected because: Third-party builders introduce external dependency overhead and fail to encapsulate application-specific anchor state models required by channel storage.
  When valid: Valid when notification schemas require standard block generation without custom state anchoring or caching abstractions.

## Risks

- Changes in Slack Block Kit wire specifications could invalidate existing serialized block structures held in cache storage.
  Mitigation: Enforce strict schema validation and bounded time-to-live expiration policies on cached anchor records.
  Owner: Engineering Team
- Tight coupling between block anchor data structures and persistence serialization could hinder transport updates.
  Mitigation: Define explicit data transfer interfaces separating raw presentation blocks from minimal cache anchor references.
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
- Design exported functions within the slack-blocks module to produce pure data objects without side effects or embedded transport client invocations.
- Ensure anchor keys and block identifier generators encapsulate predictable schemas to facilitate safe cache lookup and deterministic message updates.

## Continuation Context


Verify commands:
- Discover the workspace verification script defined in the project configuration and execute it to validate type consistency and architectural boundary compliance across all notification modules.
- Discover and run the project test suite covering notification channel formatting and anchor storage serialization.

Accept when:
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

## Enforcement

- Verified by: Automated static analysis and module import boundary checks during pull request validation
- Verified by: Peer code review verifying that message layouts are not constructed ad hoc outside the designated module
- Violation handling: Pull requests containing inline Slack Block Kit payload definitions or unauthorized module couplings are blocked from merging until refactored into the shared module.
- Exception process: Architectural exceptions require approval from notification subsystem maintainers accompanied by documented technical justification for bypassing standard block modules.