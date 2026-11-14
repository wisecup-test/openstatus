---
version: 2
name: 7f911fa1-4fb4-48ce-86d7-ea171495abe7-slack-web-api-integration-client-adoption-interactive-action-handlers-decouple-stateful-operation
description: "@slack/web-api Integration Client Adoption: Interactive Action Handlers Decouple Stateful Operation"
metadata:
  actual:
    adr_id: 7f911fa1-4fb4-48ce-86d7-ea171495abe7
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: Interactive Action Handlers Decouple Stateful Operation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/slack/**`

## Context

- Server integration boundaries interact with external messaging platforms to deliver interactive surfaces and handle user-driven actions.
- Direct synchronous execution of interactive actions received from external webhooks introduces risks of duplicate processing and replay vulnerabilities.
- The codebase establishes a unified integration pattern adopting the @slack/web-api library alongside a pending action confirmation store to coordinate view publication, state updates, and single-use action consumption.

## Problem Statement

Interactive messaging integrations require reliable coordination between outbound view publications, interactive message updates, and asynchronous user action processing. Executing stateful operations directly upon receipt of external interaction callbacks risks race conditions, duplicate execution from re-delivered webhooks, and inconsistent user feedback across distributed boundaries.

## Decision

1. MUST: Interactive action handlers MUST decouple stateful operation execution from webhook receipt by staging pending actions in a confirmation store and consuming each action record exactly once upon execution.

## Policy Block

- MUST Interactive action handlers MUST decouple stateful operation execution from webhook receipt by staging pending actions in a confirmation store and consuming each action record exactly once upon execution.

In scope:
- All server routes and modules managing interactive messaging integration, view publishing, and interaction handling across platform boundaries.
- Asynchronous interactive workflows requiring confirmation storage, expiration tracking, and status message updates.

Out of scope:
- Internal service-to-service communication within the core application boundary not interacting with external messaging providers.
- Static notification channels that do not utilize interactive blocks, user confirmation lifecycles, or published view states.

Exceptions:
- EXC-27-001: Direct webhook delivery without interactive blocks or stateful confirmation is required by external platform restrictions.

## Rationale

- Standardizing integration boundaries on @slack/web-api ensures consistent API interaction conventions, centralized error handling, and uniform payload delivery across server integration handlers.
- Decoupling interaction handlers through pending confirmation storage and single-use state consumption prevents duplicate action execution and mitigates race conditions in distributed environments.
- Updating messaging surfaces with explicit state indicators provides deterministic feedback to users while invalidating expired or executed action triggers.

## Consequences

Positive:
- Centralizes messaging boundary interactions within a dedicated client abstraction, isolating external communication from domain services.
- Guarantees single-use confirmation state consumption to prevent replay attacks and accidental duplicate execution of sensitive actions.
- Ensures deterministic message lifecycle updates that provide immediate feedback and clean visual audit trails for users.

Negative:
- Introduces runtime dependencies on external messaging API availability and platform rate limits for view and chat updates.
- Requires maintaining synchronized state storage and expiration policies for pending interactive actions.

## Alternatives

- Direct HTTP client calls to integration endpoints without client library abstraction (rejected)
  Rejected because: Direct HTTP requests require manual serialization, authentication header management, custom retry handling, and duplicated API type definitions across routes.
  When valid: Valid only in constrained runtime environments where client library dependencies cannot be loaded.
- Immediate synchronous execution of interactive actions without confirmation storage (rejected)
  Rejected because: Lacks defense against duplicate webhook deliveries, user replay actions, and state tampering during interactive confirmation flows.
  When valid: Valid only for idempotent, read-only interactive queries that carry no persistent side effects.

## Risks

- Rate limiting or transient network failures during client view publication or chat update calls
  Mitigation: Implement resilient client configuration with exponential backoff and propagate descriptive fallback errors to logging infrastructure.
  Owner: Integration Engineering Team
- Pending confirmation state leakage or resource exhaustion if expiration time-to-live settings fail
  Mitigation: Enforce strict time-to-live boundaries on persisted pending actions and implement automated cache eviction.
  Owner: Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct dedicated boundary clients with required platform credentials and configure interaction endpoints to consume pending actions deterministically upon receipt.
- Ensure all interactive payloads and confirmation objects undergo schema validation prior to persisting or consuming state within the storage layer.

## Continuation Context


Verify commands:
- Discover the test automation task in the project configuration manifest and execute integration test suites targeting external messaging routes.
- Identify the repository type checking and static analysis verification scripts and run them across all integration boundary modules.

Accept when:
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

## Enforcement

- Verified by: Continuous integration test validation suites enforcing integration contract and handler specifications.
- Verified by: Peer code review verifying adherence to library usage patterns and pending confirmation lifecycle management.
- Violation handling: Pull requests bypassing the standard integration client or omitting pending state consumption must be rejected during automated checks and code review.
- Violation handling: Identified unvalidated interactive action handlers must be flagged as high-priority integration defects and retrofitted with schema validation and confirmation storage.
- Exception process: Submit an architectural review request outlining why direct unconfirmed execution or alternate communication protocols are required.
- Exception process: Obtain formal sign-off from the integration architecture team and document the compensating controls.