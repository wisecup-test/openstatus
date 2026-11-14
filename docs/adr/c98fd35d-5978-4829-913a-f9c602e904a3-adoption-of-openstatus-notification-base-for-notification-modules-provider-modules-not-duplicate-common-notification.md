---
version: 2
name: c98fd35d-5978-4829-913a-f9c602e904a3-adoption-of-openstatus-notification-base-for-notification-modules-provider-modules-not-duplicate-common-notification
description: "Adoption of @openstatus/notification-base for Notification Modules: Provider Modules Not Duplicate Common Notification"
metadata:
  actual:
    adr_id: c98fd35d-5978-4829-913a-f9c602e904a3
    status: accepted
    candidate_group_id: c9667bbc-a93e-4be6-8512-d4d9f3810ba7
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Modules: Provider Modules Not Duplicate Common Notification

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/discord`

Applies to:
- `src/**`

## Context

- Multiple notification provider integration modules coordinate message construction and delivery.
- Inconsistent notification models across distinct provider channels introduce contract drift and redundant transformation logic.
- The workspace architecture provides internal shared modules to encapsulate foundational domain contracts and utilities.

## Problem Statement

Implementing distinct notification provider integrations independently risks duplicating payload formatting logic and fragmenting base message contracts across the codebase.

## Decision

1. MUST_NOT: Provider modules MUST NOT duplicate common notification payload contracts or base schema declarations outside @openstatus/notification-base.

## Policy Block

- MUST_NOT Provider modules MUST NOT duplicate common notification payload contracts or base schema declarations outside @openstatus/notification-base.

In scope:
- All notification integration modules and dispatchers within the notification subsystem.

Out of scope:
- Independent external client wrappers and unrelated domain services that do not deliver notifications.

## Rationale

- Centralizing shared notification contracts in @openstatus/notification-base ensures consistent message structures across channels.
- Observed evidence confirms cross-module adoption of @openstatus/notification-base for notification construction and dispatching logic.
- Consolidating common interfaces in a shared module reduces boilerplate and simplifies testing across notification providers.

## Consequences

Positive:
- Standardizes notification interfaces, shared utilities, and payload contracts across independent channel packages.
- Encapsulates core notification invariants within a shared internal module, minimizing redundant domain code.

Negative:
- Couples notification delivery providers to the internal evolution and release lifecycle of @openstatus/notification-base.
- Breaking contract changes in @openstatus/notification-base necessitate synchronized updates across all consuming provider modules.

## Alternatives

- Direct inline implementation of notification contracts within each provider module (rejected)
  Rejected because: Duplicates message formatting logic and payload contract schemas across notification channels, leading to schema drift.
  When valid: Single-channel notification prototypes with no shared infrastructure requirements.
- Generic third-party notification abstraction framework (rejected)
  Rejected because: Introduces extraneous external dependencies and misaligns with internal domain data models.
  When valid: Standardized enterprise notification pipelines routing to commercial multi-channel delivery SaaS.

## Risks

- Upstream contract modifications in the base package breaking downstream provider delivery pipelines.
  Mitigation: Maintain strict semantic versioning and regression testing suites for shared notification exports.
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
- Notification provider implementations must isolate provider-specific payload formatting from shared notification domain logic.
- Consumers must align data transformation boundaries with the schemas and interfaces exposed by the base notification package.

## Continuation Context


Verify commands:
- Discover and execute repository dependency boundary and module linting scripts.
- Discover and run test suites targeting notification provider modules.

Accept when:
- Static analysis confirms notification integration modules import shared notification primitives from @openstatus/notification-base.
- All automated test suites and contract validations for notification delivery pass without structural dependency violations.

## Enforcement

- Verified by: Continuous integration dependency checks and automated module boundary validation rules.
- Verified by: Peer code review during pull request evaluations for notification provider packages.
- Violation handling: Pull requests that bypass shared notification primitives or implement duplicate base contracts are blocked from merging.
- Exception process: Exceptions require architectural review and formal approval documented in an architectural exception record.