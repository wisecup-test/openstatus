---
version: 2
name: f1d0de46-3a06-46a8-af2f-86ae95f71ce6-adoption-of-openstatus-notification-base-for-notification-handlers-notification-delivery-services-import-extend-shared
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Import Extend Shared"
metadata:
  actual:
    adr_id: f1d0de46-3a06-46a8-af2f-86ae95f71ce6
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Import Extend Shared

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/email`

Applies to:
- `src/**`

## Context

- The workspace maintains notification delivery modules across distinct transport channels such as email.
- Inconsistent notification processing and unvalidated payload ingestion across disparate channels can cause silent delivery failures and maintenance fragmentation.
- The codebase establishes internal foundation modules to provide shared domain logic and standard interfaces across services.

## Problem Statement

Without a standardized base module, individual notification transport packages risk implementing disparate event schemas, inconsistent payload validation routines, and divergent dispatch behaviors, complicating cross-channel maintenance and reliability.

## Decision

1. MUST: Notification delivery services MUST import and extend the shared @openstatus/notification-base module to implement notification handling and dispatch workflows.

## Policy Block

- MUST Notification delivery services MUST import and extend the shared @openstatus/notification-base module to implement notification handling and dispatch workflows.

In scope:
- Applies to all notification handler packages and transport dispatchers within the workspace.
- Applies when defining incoming notification event consumption and dispatch workflows.

Out of scope:
- Does not apply to independent external webhook receivers that operate outside the workspace notification architecture.
- Does not apply to client-side notification presentation components.

Exceptions:
- EXC-20-001: A notification handler requires experimental protocol testing in an isolated test harness prior to workspace integration.

## Rationale

- Adopting @openstatus/notification-base centralizes common notification handling primitives, ensuring uniform structure across delivery mechanisms.
- Mandating safeParse payload validation at the handler boundary guarantees invalid event structures are trapped before dispatch operations occur.
- Shared foundation modules reduce code duplication and enforce consistency across monorepo notification packages.

## Consequences

Positive:
- Enforces a uniform notification handling interface across distinct transport channels.
- Guaranteed payload schema validation prevents malformed notifications from triggering runtime errors during message delivery.
- Simplifies maintenance through centralized domain abstractions in workspace modules.

Negative:
- Creates a direct architectural dependency on internal workspace foundation packages.
- Requires schema updates in upstream modules whenever notification payload structures evolve.

## Alternatives

- Decentralized notification handling with independent parsing logic per transport channel (rejected)
  Rejected because: Leads to schema divergence, redundant parsing implementations, and increased defect rates across channels.
  When valid: Valid only in standalone single-channel prototypes without shared workspace infrastructure.
- Direct dependency on database schema entities without intermediate notification base abstraction (rejected)
  Rejected because: Tightly couples transport mechanisms directly to persistence models rather than domain event contracts.
  When valid: Valid in micro-services where notifications are written directly to database queues without channel-specific handling.

## Risks

- Breaking changes in the foundation module could impact all notification channels simultaneously.
  Mitigation: Enforce contract-level automated testing across all consuming notification packages during integration runs.
  Owner: Engineering team
- Uncaught schema parsing rejections could lead to unacknowledged or dropped notification jobs.
  Mitigation: Ensure notification handlers trap safeParse failures and route invalid events to a dead-letter or error logging mechanism.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct notification handler modules by importing shared base interfaces and applying schema-driven parsing to payload inputs.
- Verify that all internal workspace module imports resolve cleanly through the repository build system.

## Continuation Context


Verify commands:
- Discover and run the project's dependency analysis script to verify that notification handlers reference the approved shared foundation module.
- Discover and run the workspace test suite covering notification modules to ensure payload validation and event handling pass.
- Discover and run the repository typecheck script to ensure all notification interface contracts are satisfied.

Accept when:
- All notification packages successfully build and pass typechecking against the shared foundation contracts.
- Unit and integration tests for notification handlers succeed, confirming payload validation correctly accepts valid schemas and rejects invalid data.
- Static analysis confirms no notification package imports unapproved alternate notification handling libraries.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository validation suites.
- Verified by: Peer code review for pull requests modifying notification packages.
- Violation handling: Pull requests violating architectural constraints or bypassing the foundation module will be blocked until rectified.
- Violation handling: Static analysis and test failures will prevent automated deployment.
- Exception process: Submit an architectural exception request detailing the rationale and proposed timeline for alignment, subject to platform team review.