---
version: 2
name: 5cb15620-ab1a-4999-a929-cac147ddbf73-adoption-of-openstatus-notification-base-core-library-notification-handlers-not-bypass-schema-validation
description: "Adoption of @openstatus/notification-base Core Library: Notification Handlers Not Bypass Schema Validation"
metadata:
  actual:
    adr_id: 5cb15620-ab1a-4999-a929-cac147ddbf73
    status: accepted
    candidate_group_id: 134a167d-ded5-4810-8ca0-70ed27e07461
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base Core Library: Notification Handlers Not Bypass Schema Validation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/grafana-oncall`

Applies to:
- `src/**`

## Context

- The notification subsystem coordinates alert dispatch from monitor events to multiple external notification providers and incident response platforms.
- Inconsistent payload structures and ad-hoc notification formatting across third-party providers risk silent delivery failures and unvalidated data transmission.
- The codebase establishes internal shared libraries including @openstatus/notification-base to provide consistent contracts and utilities across individual notification handler packages.
- Alert state transitions representing down, degraded, and recovered states require strict schema validation before constructing provider-specific payloads.

## Problem Statement

Without a unified core notification module and standardized payload parsing, individual notification integrations implement fragmented dispatch logic, duplicated data parsing, and inconsistent error handling when transforming monitor health events into external provider alerts. This fragmentation increases maintenance overhead and allows malformed notification payloads to traverse external client boundaries undetected.

## Decision

1. MUST_NOT: Notification handlers MUST NOT bypass schema validation or transmit unvalidated event data directly to external notification endpoints.

## Policy Block

- MUST_NOT Notification handlers MUST NOT bypass schema validation or transmit unvalidated event data directly to external notification endpoints.

In scope:
- All notification provider integration packages handling monitor alert dispatch.
- Modules transforming internal monitor status events into external third-party webhook payloads.

Out of scope:
- Internal monitoring engine checks that do not emit external notification events.
- Direct database persistence operations unrelated to external alert delivery.

## Rationale

- Adopting @openstatus/notification-base centralizes notification handler contracts across the monorepo workspace, ensuring unified integration boundaries.
- Enforcing runtime schema parsing on both inbound notification event data and outbound provider payloads catches malformed monitor data before external dispatch occurs.
- Standardizing provider payload construction for alerting and recovery states ensures predictable telemetry and upstream link references across all supported notification targets.

## Consequences

Positive:
- Guarantees strict runtime validation of incoming notification data and outgoing third-party payloads.
- Harmonizes notification handling across provider packages through reusable shared module architecture.
- Prevents malformed monitor status transitions from reaching external webhook endpoints.

Negative:
- Introduces build-time and runtime coupling between notification provider packages and the shared internal base module.
- Requires defining and maintaining synchronized schema definitions for every supported external provider format.

## Alternatives

- Ad-hoc inline notification dispatching per provider package without shared base dependencies (rejected)
  Rejected because: Leads to duplicated payload validation logic, inconsistent alert state mapping, and increased defect rates across provider integrations.
  When valid: Prototyping standalone notification handlers outside the workspace repository architecture.
- Centralized monolithic notification service handling all provider formatting internally (rejected)
  Rejected because: Couples all provider dependencies into a single deployment artifact and prevents modular development of isolated notification packages.
  When valid: Small monolithic applications supporting only a single static alert destination.

## Risks

- Breaking changes in the shared @openstatus/notification-base package could cascade across all consuming notification provider handlers.
  Mitigation: Enforce contract compatibility through workspace type checking and automated regression test suites prior to version bumping.
  Owner: Notification Platform Engineering Team
- External notification provider API changes may invalidate runtime schema definitions causing payload rejections.
  Mitigation: Maintain integration tests against external provider schemas and isolate schema definitions within dedicated schema modules.
  Owner: Integration Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct provider notification payloads by parsing validated status event records and mapping monitor attributes to alert titles, messages, and state fields.
- Isolate provider-specific schema definitions in localized schema modules and import common utilities from workspace packages.

## Continuation Context


Verify commands:
- Discover the test runner command from the repository build manifest and execute integration tests for the notification handler.
- Discover the workspace type-checking command from the project configuration and validate schema contract compliance.
- Discover the linting and static analysis command from the root configuration and verify module boundary adherence.

Accept when:
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

## Enforcement

- Verified by: Automated continuous integration pipelines executing workspace test and type-check commands.
- Verified by: Mandatory peer code reviews checking conformance to shared base module architecture and schema parsing rules.
- Violation handling: Pull requests failing schema validation tests or omitting base module contracts are blocked from merging.
- Violation handling: Code reviews require refactoring direct unvalidated network calls to use schema parse boundaries.
- Exception process: Exceptions require documented justification submitted to the architecture review team explaining why custom transport or unvalidated payloads are required.