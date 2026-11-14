---
version: 2
name: 47e91675-7dc5-4295-9024-c5e102c2d24f-adoption-of-openstatus-notification-base-for-notification-dispatch-modules-notification-providers-encapsulate-destination-specific-payload
description: "Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Providers Encapsulate Destination Specific Payload"
metadata:
  actual:
    adr_id: 47e91675-7dc5-4295-9024-c5e102c2d24f
    status: accepted
    candidate_group_id: 2cc45944-5dce-4b8e-9587-6ebcd38a2363
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Providers Encapsulate Destination Specific Payload

Status: proposed
Date: 2025-02-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/pagerduty`

Applies to:
- `src/**`

## Context

- Outbound notification delivery requires communicating with disparate third-party event ingestion endpoints while maintaining consistent alert lifecycle handling.
- Notification integration packages require standard base interfaces and shared contracts to avoid diverging data mapping and execution patterns across provider implementations.
- The codebase isolates external notification providers into dedicated packages that consume shared internal modules for base notification workflows.

## Problem Statement

Integrating third-party alerting platforms without a shared foundation leads to fragmented payload parsing, inconsistent error handling, and duplicate notification pipeline logic across individual provider packages.

## Decision

1. SHOULD: Notification providers SHOULD encapsulate destination-specific payload schema definitions within local provider configuration schemas and verify event payloads prior to network transmission.

## Policy Block

- SHOULD Notification providers SHOULD encapsulate destination-specific payload schema definitions within local provider configuration schemas and verify event payloads prior to network transmission.

In scope:
- All notification provider modules and integration packages delivering alert events to external services
- Modules that ingest alert data and dispatch outbound notifications across supported channels

Out of scope:
- Internal data persistence layer operations that do not dispatch notifications
- Third-party integrations that operate outside of the alerting and notification delivery domain

## Rationale

- Standardizing on @openstatus/notification-base establishes a common contract for notification dispatch across distinct provider packages.
- Validating incoming payload structures with schema parsing ensures that malformed notification payloads are caught prior to initiating network requests.
- Encapsulating provider-specific payload schemas alongside base notification interfaces keeps provider packages self-contained while conforming to shared architectural contracts.

## Consequences

Positive:
- Guarantees structural uniformity across notification providers by adhering to shared base module abstractions.
- Prevents runtime failures during remote event dispatch by enforcing schema parsing on notification data prior to transmission.
- Simplifies onboarding of new notification channels through reusable base contracts.

Negative:
- Couples notification provider implementations to changes in the shared base module interface.
- Requires maintaining strict schema alignment between shared database models, notification payloads, and provider-specific schemas.

## Alternatives

- Decentralized independent notification packages without shared base abstractions (rejected)
  Rejected because: Allowing each notification package to independently define dispatch logic and payload handling causes implementation divergence and increases maintenance overhead across alert channels.
  When valid: When notification providers share zero behavioral overlap or require fundamentally incompatible execution models.
- Monolithic notification engine handling all alert channels in a single module (rejected)
  Rejected because: Consolidating all notification channel logic into a single monolithic package creates high coupling and complicates isolated testing and deployment of individual provider integrations.
  When valid: When the system only supports a single notification channel with no anticipated expansion.

## Risks

- Breaking interface updates in @openstatus/notification-base could disrupt multiple notification providers simultaneously
  Mitigation: Establish workspace contract tests that verify all consuming notification provider packages against base interface changes
  Owner: engineering team
- External alerting endpoint API changes could invalidate schema assumptions and fail event enqueueing
  Mitigation: Maintain dedicated schema definitions and automated integration verification suites for external endpoint payloads
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
- Notification integration packages should isolate external client HTTP transmission logic from internal data transformation logic.
- Ensure schema definitions for incoming notification data and outgoing third-party events remain distinct to prevent schema leakage.

## Continuation Context


Verify commands:
- Discover and execute the project workspace test suite covering notification packages.
- Discover and execute the repository type-checking and schema validation verification scripts across notification modules.

Accept when:
- All notification packages pass workspace verification scripts without schema parsing or type errors.
- Notification payload parsing and outbound event payload schemas validate successfully during automated test execution.

## Enforcement

- Verified by: Automated continuous integration test and lint validation pipelines
- Verified by: Architectural peer code review verifying compliance with shared base contracts
- Violation handling: Rejection of pull requests that bypass the base notification library or introduce unvalidated payload handling
- Violation handling: Required remediation to align notification provider logic with shared base schemas before deployment
- Exception process: Explicit technical debt justification submitted to and approved by the lead architects for specialized transport mechanisms that cannot use standard base interfaces.