---
version: 2
name: 2e9cb41f-6ce9-40e1-adf0-a4b90b3d08bf-adoption-of-openstatus-notification-base-for-notification-handlers-notification-message-formatting-across-lifecycle-states
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Message Formatting Across Lifecycle States"
metadata:
  actual:
    adr_id: 2e9cb41f-6ce9-40e1-adf0-a4b90b3d08bf
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Message Formatting Across Lifecycle States

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/twillio-sms`

Applies to:
- `src/**`

## Context

- The system dispatches operational alerts across external communication channels based on monitor status transitions.
- Notification delivery requires channel-specific message construction, payload parsing, and external API client interaction.
- Consolidating common notification mechanics into a shared internal module prevents divergence across provider packages.

## Problem Statement

Dispersed notification delivery logic without a shared base module leads to duplicated message formatting, divergent payload handling, and inconsistent error recovery across distinct communication channels.

## Decision

1. SHOULD: Notification message formatting across lifecycle states such as monitor failure, degradation, and recovery SHOULD be encapsulated within the notification dispatch handler adhering to @openstatus/notification-base contracts.

## Policy Block

- SHOULD Notification message formatting across lifecycle states such as monitor failure, degradation, and recovery SHOULD be encapsulated within the notification dispatch handler adhering to @openstatus/notification-base contracts.

In scope:
- Notification handler packages and dispatch services responsible for outbound alert delivery across communication channels.
- Workspace modules implementing provider integrations governed by internal notification contracts.

Out of scope:
- Internal database query layers and schema definition packages that do not directly manage notification dispatch.
- Core monitoring evaluation engines that trigger alerts without managing channel-specific message formatting or delivery.

## Rationale

- Adopting @openstatus/notification-base establishes a centralized, reusable foundation for all outbound notification integrations across communication channels.
- Encapsulating notification dispatch within a standardized module prevents code duplication between provider implementations and enforces consistent contract handling.
- Pairing the base module with structured payload validation and environment-backed secrets isolation minimizes runtime dispatch failures and credential leakage.

## Consequences

Positive:
- Standardizes outbound alert processing and error handling across distinct notification transport providers.
- Reduces boilerplate in provider-specific integration packages by leveraging shared notification contracts and interfaces.
- Enforces decoupling between notification trigger logic and channel-specific transport implementations.

Negative:
- Introduces a shared dependency on @openstatus/notification-base across all notification packages, requiring careful version alignment.
- Changes to base notification interfaces necessitate synchronized updates and regression verification across provider implementations.

## Alternatives

- Independent standalone notification scripts without shared base module abstraction (rejected)
  Rejected because: Results in redundant message formatting logic, disparate payload parsing implementations, and inconsistent error handling across notification channels.
  When valid: Ad-hoc prototypes or single-channel proof-of-concept integrations where long-term maintainability and consistency are not required.
- Direct dependency on third-party multi-channel notification SaaS SDKs (rejected)
  Rejected because: Introduces external vendor lock-in, recurring operational subscription costs, and reduced flexibility for custom alerting logic and self-hosted deployments.
  When valid: Projects requiring turnkey hosted notification routing where internal engineering bandwidth cannot support custom channel providers.

## Risks

- Breaking changes in @openstatus/notification-base interfaces could disrupt multiple notification providers simultaneously.
  Mitigation: Enforce contract stability through strict schema validation, semantic versioning of workspace packages, and automated integration test suites across all provider modules.
  Owner: Platform Engineering
- Failure or latency spikes in external transport endpoints during alert dispatch can stall notification pipelines.
  Mitigation: Implement execution timeouts, asynchronous dispatch queues, and resilient retry mechanisms with backoff for outbound client requests.
  Owner: Infrastructure Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement channel-specific notification providers as modular packages extending @openstatus/notification-base, consuming validated notification payloads and mapping them into provider-compatible request structures.
- Ensure all external endpoint secrets and credentials required by notification handlers are injected via environment configuration rather than hardcoded in package source files.

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis and type verification script from the repository manifest to verify @openstatus/notification-base contract conformance.
- Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior.

Accept when:
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

## Enforcement

- Verified by: Continuous integration pipeline verifying static typing, package boundary conformance, and test passage.
- Verified by: Mandatory architectural and code review on pull requests modifying or adding notification handlers.
- Violation handling: Pull requests bypassing @openstatus/notification-base or hardcoding external credentials will be blocked from merging.
- Violation handling: Existing non-conforming notification handlers must be refactored to align with the base module architecture.
- Exception process: Exceptions require documented justification submitted via an architectural exception request and approval by the platform engineering lead.