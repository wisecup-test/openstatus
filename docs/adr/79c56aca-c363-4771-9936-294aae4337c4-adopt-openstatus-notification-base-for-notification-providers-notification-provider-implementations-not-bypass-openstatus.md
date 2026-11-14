---
version: 2
name: 79c56aca-c363-4771-9936-294aae4337c4-adopt-openstatus-notification-base-for-notification-providers-notification-provider-implementations-not-bypass-openstatus
description: "Adopt @openstatus/notification-base for Notification Providers: Notification Provider Implementations Not Bypass Openstatus"
metadata:
  actual:
    adr_id: 79c56aca-c363-4771-9936-294aae4337c4
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: Notification Provider Implementations Not Bypass Openstatus

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/ntfy`

Applies to:
- `src/**`

## Context

- Notification integrations require consistent contracts and shared definitions across notification providers.
- Notification provider implementations depend on internal workspace packages including @openstatus/notification-base for shared base functionality.
- Incoming provider payload data requires parsing and validation prior to dispatching external HTTP requests.

## Problem Statement

Notification providers implemented in isolation risk divergent interfaces, duplicated transport logic, and inconsistent data validation protocols. A standardized shared module is required to structure notification implementations across provider integrations.

## Decision

1. MUST_NOT: Notification provider implementations MUST NOT bypass @openstatus/notification-base contracts by implementing incompatible ad-hoc notification handlers.

## Policy Block

- MUST_NOT Notification provider implementations MUST NOT bypass @openstatus/notification-base contracts by implementing incompatible ad-hoc notification handlers.

In scope:
- All notification provider packages and modules responsible for dispatching external notifications.
- Integration packages implementing specific notification delivery channels.

Out of scope:
- Core database schema definitions and shared utilities that do not handle notification delivery.
- Consumers of notifications that only trigger delivery without implementing provider adapters.

Exceptions:
- EXC-20-001: A notification integration requires transport protocols not supported by base contracts

## Rationale

- Static analysis identifies @openstatus/notification-base as an adopted core library within notification delivery modules.
- Standardizing on a shared internal module establishes uniform integration contracts across provider implementations.
- Pairing the shared base package with schema parsing ensures strict boundary validation before triggering external HTTP calls.

## Consequences

Positive:
- Enforces consistent structure and contracts across disparate notification delivery providers.
- Centralizes shared notification domain logic and reduces code duplication across provider packages.
- Ensures data validation occurs before external network boundaries are reached.

Negative:
- Creates tight coupling between notification provider packages and @openstatus/notification-base internal release cycles.
- Requires provider maintainers to keep abreast of changes and updates to the internal base package.

## Alternatives

- Independent standalone implementations for each notification provider without shared base modules (rejected)
  Rejected because: Leads to code duplication, divergent payload handling, and inconsistent error semantics across providers
  When valid: When providers share zero architectural conventions and operate in completely separate codebases
- Consolidated monolithic notification service without modular provider packages (rejected)
  Rejected because: Limits modularity and prevents isolated testing or deployment of individual notification adapters
  When valid: When only a single notification channel will ever be supported

## Risks

- Breaking changes in @openstatus/notification-base can cascade to all downstream notification provider packages
  Mitigation: Establish strict semantic versioning and automated integration test coverage across provider workspaces
  Owner: engineering team
- Incomplete validation of dynamic payload data could cause runtime dispatch failures in external delivery calls
  Mitigation: Enforce strict schema parsing on notification payload data prior to network operations
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
- Discover the workspace package manifest to import @openstatus/notification-base and verify exported base types and functions.
- Ensure schema parsing logic validates incoming dynamic payload structures prior to triggering outbound HTTP dispatch.

## Continuation Context


Verify commands:
- Discover and run the project workspace type-check script to verify module interface compatibility.
- Discover and run the test suite for notification provider packages to validate delivery dispatch behavior.
- Discover and run the repository linter to verify dependency boundary compliance across internal packages.

Accept when:
- The notification provider compiles with zero type errors when referencing @openstatus/notification-base.
- Payload validation succeeds for conformant data and rejects invalid inputs prior to external network requests.
- All discovered package test suites pass successfully.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace compilation and test suites.
- Verified by: Peer code review verifying adherence to shared provider patterns and schema validation.
- Violation handling: Pull requests omitting @openstatus/notification-base or payload validation in notification providers will be blocked from merging.
- Violation handling: Flagged violations must be refactored to align with the shared provider architecture before approval.
- Exception process: Submit an architectural review request outlining the technical constraints preventing adoption of @openstatus/notification-base.
- Exception process: Obtain explicit sign-off from the technical lead before implementing standalone provider logic.