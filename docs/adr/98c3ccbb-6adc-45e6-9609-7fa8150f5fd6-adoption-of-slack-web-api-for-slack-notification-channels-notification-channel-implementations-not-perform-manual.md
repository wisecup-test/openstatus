---
version: 2
name: 98c3ccbb-6adc-45e6-9609-7fa8150f5fd6-adoption-of-slack-web-api-for-slack-notification-channels-notification-channel-implementations-not-perform-manual
description: "Adoption of @slack/web-api for Slack Notification Channels: Notification Channel Implementations Not Perform Manual"
metadata:
  actual:
    adr_id: 98c3ccbb-6adc-45e6-9609-7fa8150f5fd6
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: Notification Channel Implementations Not Perform Manual

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/channels/**`

## Context

- The subscription subsystem dispatches notifications and status reports across multiple subscriber channels, including team workspaces.
- Delivering structured status reports requires dynamic chat updates and structured message layout blocks rather than plain text broadcasts.
- Direct HTTP integrations with third-party messaging platforms lead to duplicated network handling, manual authentication headers, and unverified payload schemas.

## Problem Statement

Managing Slack channel notifications through ad-hoc HTTP requests creates schema drift, inconsistent error handling, and manual payload construction overhead. An official, typed library is required to govern all outbound Slack API communications, ensure valid block structure formatting, and normalize response handling.

## Decision

1. MUST_NOT: Notification channel implementations MUST NOT perform manual HTTP requests to Slack Web API endpoints when equivalent client methods exist within `@slack/web-api`.

## Policy Block

- MUST_NOT Notification channel implementations MUST NOT perform manual HTTP requests to Slack Web API endpoints when equivalent client methods exist within `@slack/web-api`.

In scope:
- Notification delivery modules responsible for formatting and sending messages to Slack channels.
- Modules constructing and assembling message blocks for Slack status distributions.

Out of scope:
- Notification delivery channels targeting alternative mediums such as email or generic webhooks.
- Modules handling external incoming webhook ingestion rather than outbound platform dispatching.

## Rationale

- Consolidating Slack interactions around `@slack/web-api` ensures that message updates and block formatting conform to typed contracts provided by the platform maintainer.
- Abstracting Slack communication behind a standardized client simplifies testing and decouples business logic from low-level network and authentication protocols.
- Uniform error surface handling allows subscriber state management to accurately flag terminal errors and unsubscribe invalid destinations.

## Consequences

Positive:
- Provides official, type-safe interfaces for constructing message blocks and dispatching chat updates.
- Centralizes error structures returned by the external service, enabling consistent handling of transient and terminal delivery failures.
- Eliminates redundant implementation of platform-specific authentication and endpoint routing logic.

Negative:
- Introduces an external dependency and runtime coupling to the third-party client library release cadence and deprecation cycles.
- Increases package bundle size within notification delivery modules compared to raw endpoint requests.

## Alternatives

- Direct HTTP requests to Slack REST API endpoints via a generic network client (rejected)
  Rejected because: Requires custom maintenance of authentication headers, payload serialization, rate limiting, and response parsing without the static typing guarantees provided by an official client library.
  When valid: Permissible only in severely constrained environments where third-party package footprints are prohibited and interactions are limited to single static endpoints.
- Unauthenticated incoming webhooks for notification dispatching (rejected)
  Rejected because: Incoming webhooks do not support granular chat updates, channel querying, or dynamic message block interactions required for status tracking.
  When valid: Acceptable for simple fire-and-forget notifications where message modification and interactive block updates are not required.

## Risks

- Vendor SDK updates may introduce breaking changes to chat update method signatures or block layout structures.
  Mitigation: Lock exact client versions in the repository resolution artifact and execute channel regression tests prior to version upgrades.
  Owner: Engineering Team
- Unhandled vendor API errors or rate limiting may cause notification delivery failures and misclassified subscriber statuses.
  Mitigation: Implement error classification logic that parses platform error codes to handle terminal subscription errors distinctly from transient communication failures.
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
- Separate message layout construction and block formatting into dedicated helper modules to keep channel delivery handlers focused on dispatching and state updates.
- Extract and inspect error response codes from client method failures to identify permanent subscriber invalidations versus temporary network faults.

## Continuation Context


Verify commands:
- Discover the repository dependency management scripts and execute the static analysis suite to verify that all Slack platform interactions route exclusively through the approved client module.
- Locate and execute the test runner for the channel delivery test suite to verify that message block formatting and chat update operations handle platform responses as expected.

Accept when:
- Static analysis confirms all Slack platform communication is routed through the approved SDK client without direct HTTP requests to Slack endpoints.
- Automated test suites for channel dispatching pass with verified handling of both successful message updates and terminal API failure responses.

## Enforcement

- Verified by: Automated static analysis verifying dependency import boundaries and prohibiting raw HTTP calls to external vendor endpoints.
- Verified by: Peer code review of notification channel implementations.
- Violation handling: Static analysis and linting checks fail automated build gates if unapproved communication paths are introduced.
- Violation handling: Pull requests with manual endpoint calls are blocked pending refactoring to the approved client interfaces.
- Exception process: Exceptions require submission of an architectural review request demonstrating why the standard client cannot satisfy the integration requirement.
- Exception process: Approved exceptions must document the custom transport mechanism and undergo security review for credential handling.