---
version: 2
name: d016098f-b68f-4ae9-bacf-3d8ccfbdea38-adoption-of-openstatus-notification-base-for-outbound-webhook-integration-incoming-notification-payload-data-parsed-validated
description: "Adoption of @openstatus/notification-base for Outbound Webhook Integration: Incoming Notification Payload Data Parsed Validated"
metadata:
  actual:
    adr_id: d016098f-b68f-4ae9-bacf-3d8ccfbdea38
    status: accepted
    candidate_group_id: d10cb657-9b38-4b0f-86db-ef19fe31223b
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Outbound Webhook Integration: Incoming Notification Payload Data Parsed Validated

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/grafana-oncall`

Applies to:
- `src/**`

## Context

- Incident alert systems require reliable and predictable delivery of notification payloads across external client boundaries.
- External incident response platforms enforce specific JSON payload formats that require strict adherence to prevent dropped notifications.
- Adopting @openstatus/notification-base and explicit schema parsing isolates external integration contracts and prevents malformed event data from propagating to downstream incident systems.

## Problem Statement

External webhook notification endpoints require specific payload structures and metadata, but incoming monitoring events arrive as heterogeneous data structures. Without standardized schema validation and boundary dispatch patterns, external integrations risk sending malformed JSON payloads, causing silent alert delivery failures during critical incidents.

## Decision

1. MUST: All incoming notification payload data MUST be parsed and validated through designated schema parsers such as GrafanaOncallSchema before constructing external request representations.

## Policy Block

- MUST All incoming notification payload data MUST be parsed and validated through designated schema parsers such as GrafanaOncallSchema before constructing external request representations.

In scope:
- Packages and modules responsible for formatting and dispatching notification events to external incident management webhooks.
- Outbound client boundaries integrating external alert delivery services.

Out of scope:
- Internal inter-process communication that does not traverse external network boundaries.
- In-memory event buses and database persistence layers within the core notification service.

## Rationale

- Enforcing schema validation via GrafanaOncallSchema and GrafanaOncallPayload ensures runtime contract safety before network transmission.
- Using @openstatus/notification-base standardizes integration mechanics across notification packages while keeping outbound HTTP clients minimal and predictable.
- Explicit serialization to JSON with proper HTTP content headers guarantees compatibility with external receiver specifications.

## Consequences

Positive:
- Guarantees that only structurally verified payloads reach external notification receivers.
- Centralizes payload transformation and schema validation logic within dedicated notification packages.
- Reduces alert delivery failure rates due to malformed payload fields or missing attributes.

Negative:
- Introduces runtime computational overhead for schema parsing on every outbound notification dispatch.
- Requires codebase modifications and schema synchronization whenever external webhook specifications evolve.

## Alternatives

- Direct unvalidated JSON construction and transmission via HTTP client (rejected)
  Rejected because: Bypassing schema parsing introduces silent contract drift and exposes external webhook endpoints to malformed or incomplete payload fields.
  When valid: Trivial development prototypes where contract stability and delivery guarantees are not required.
- Proprietary vendor SDK integration for incident management dispatch (rejected)
  Rejected because: Adds heavy external dependencies to notification packages when lightweight schema-validated HTTP webhooks satisfy delivery requirements.
  When valid: Integrations requiring bidirectional synchronization or stateful streaming APIs not supported by standard webhooks.

## Risks

- External service webhook schemas may change upstream without notification, causing schema parse rejections.
  Mitigation: Implement integration contract tests against sample payloads and track upstream webhook specification changes.
  Owner: engineering team
- Runtime parsing of large notification payloads may incur latency during high-frequency alert storms.
  Mitigation: Keep schema definitions focused and minimal, avoiding deep nested transformations during alert delivery.
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
- Structure notification handlers to ingest raw event strings, apply schema parsing early in the pipeline, map valid attributes into external schema payloads, and dispatch via outbound HTTP calls.
- Handle dispatch network failures and non-successful HTTP status responses gracefully with appropriate retry and error logging policies defined by the notification runtime.

## Continuation Context


Verify commands:
- Discover the workspace test execution script in the project manifest and execute the test suite covering notification integration modules.
- Discover the project static type checking and linting tasks and run them across all notification package boundaries.
- Inspect the project dependency lock artifact to verify resolved package integrity and run the automated contract verification suite.

Accept when:
- All outbound notification payload schema validation tests pass without runtime parsing errors.
- HTTP dispatch mock tests confirm external webhook requests are correctly structured, serialized, and transmitted.
- Type checking and boundary contract verification suites execute with zero diagnostics across notification modules.

## Enforcement

- Verified by: Automated unit and integration test suites executed in continuous integration pipelines.
- Verified by: Pull request code reviews focusing on outbound client boundary contracts and schema parsing.
- Violation handling: Build and test step failures for unvalidated outbound payload dispatches.
- Violation handling: Immediate pull request rejection if external HTTP dispatch bypasses schema validation.
- Exception process: Formal review and approval by the platform architecture team documented with an architectural decision exception record.