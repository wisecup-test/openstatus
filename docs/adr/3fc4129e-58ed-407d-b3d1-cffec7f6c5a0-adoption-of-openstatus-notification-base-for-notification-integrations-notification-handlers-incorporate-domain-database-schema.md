---
version: 2
name: 3fc4129e-58ed-407d-b3d1-cffec7f6c5a0-adoption-of-openstatus-notification-base-for-notification-integrations-notification-handlers-incorporate-domain-database-schema
description: "Adoption of @openstatus/notification-base for Notification Integrations: Notification Handlers Incorporate Domain Database Schema"
metadata:
  actual:
    adr_id: 3fc4129e-58ed-407d-b3d1-cffec7f6c5a0
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Notification Handlers Incorporate Domain Database Schema

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/bird-whatsapp`

Applies to:
- `src/**`

## Context

- Notification delivery provider modules require a uniform interface contract to handle outbound notification events from core application services without duplicating provider lifecycle abstractions.
- The internal workspace library @openstatus/notification-base provides shared foundational definitions and contracts for notification delivery adapters.
- Notification providers isolate provider-specific dispatch logic, payload deserialization, and external client requests from central notification orchestration pipelines.
- Notification dispatch components consume database schema bindings and validated environment variables to ensure secure execution and verified payload delivery.

## Problem Statement

Decentralized notification delivery providers risk architectural drift and duplicated dispatch logic if each notification provider package independently implements notification handling contracts, payload normalization, and lifecycle management. A centralized foundational library is required to establish consistent contracts across all provider integration modules while enabling provider packages to focus solely on external client communication and payload mapping.

## Decision

1. MAY: Notification handlers MAY incorporate domain database schema models to map standardized dispatch records to external provider message payloads.

## Policy Block

- MAY Notification handlers MAY incorporate domain database schema models to map standardized dispatch records to external provider message payloads.

In scope:
- Notification integration packages and provider delivery modules implementing outbound alerting or messaging.
- Components handling incoming notification dispatch events requiring payload parsing and external service delivery.

Out of scope:
- Core database migration and schema definition modules that do not perform notification delivery.
- Internal utility libraries that have no dependency on notification routing or external messaging protocols.

## Rationale

- Adopting @openstatus/notification-base provides a single source of truth for notification handler contracts, reducing architectural divergence across diverse messaging adapters.
- Centralizing notification primitives ensures consistent error handling, input validation boundaries, and observability across all outbound channels.
- Separating base contracts into a shared internal library decouples individual provider transport implementations from internal orchestration workflows.

## Consequences

Positive:
- Establishes consistent architectural structure and delivery contracts across disparate notification provider integrations.
- Eliminates redundant contract definitions and duplicated deserialization boilerplate across provider packages.
- Centralizes governance and evolution of notification domain primitives across workspace modules.

Negative:
- Couples individual notification provider packages to the internal shared library release lifecycle.
- Changes to base notification interfaces require coordinated verification and potential updates across all dependent provider packages.
- Adds indirection that may introduce overhead when onboarding simple providers with non-standard delivery requirements.

## Alternatives

- Independent standalone implementations for each notification provider without a shared base package (rejected)
  Rejected because: Leads to fragmented notification handling logic, duplicated payload validation schemas, and inconsistent delivery contracts across channels
  When valid: Valid only in single-provider architectures where multi-channel extensibility is not required
- Monolithic single-package notification handler implementing all external provider integrations inline (rejected)
  Rejected because: Creates high blast radius where changes to one provider transport risk destabilizing other notification channels
  When valid: Valid in microservices with only one external messaging channel

## Risks

- Breaking changes in the shared base module can propagate across all dependent notification delivery packages
  Mitigation: Enforce automated integration testing suites and contract compatibility validation across all workspace packages prior to merging base module updates
  Owner: engineering team
- Third-party external provider API rate limits or authentication failures could cause unhandled dispatch rejections
  Mitigation: Implement resilient response parsing and standard error categorization derived from base contracts
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
- When authoring a notification provider adapter, import base contracts from the shared package and implement provider-specific request construction within an isolated dispatch function.
- Ensure all environment configurations required for external client credentials and workspace identifiers are validated before executing network requests.

## Continuation Context


Verify commands:
- Discover and run the repository build validation script to ensure type-checking and interface conformance with the shared base package.
- Discover and execute the project test runner to validate notification dispatch handling and payload transformation behavior.
- Discover and execute the workspace linter to confirm adherence to package boundary and import restrictions.

Accept when:
- All notification provider modules successfully compile and satisfy interface contracts defined in @openstatus/notification-base.
- External dispatch execution tests pass with valid payload assertions and schema validation checks.
- Dependency boundaries and package resolution checks succeed without unresolved imports.

## Enforcement

- Verified by: Automated continuous integration pipeline running repository build and test discovery scripts on pull requests.
- Verified by: Peer architectural review verifying adherence to base library contracts and payload validation standards.
- Violation handling: Pull requests bypassing shared base contracts or introducing unvalidated external client dispatches will be blocked from merge.
- Violation handling: Violations identified during code review must be refactored to consume shared package interfaces before deployment.
- Exception process: Architectural review submission documenting why a custom provider cannot conform to the shared base library interface.
- Exception process: Temporary exceptions require an approved migration timeline and tracked task identifier.