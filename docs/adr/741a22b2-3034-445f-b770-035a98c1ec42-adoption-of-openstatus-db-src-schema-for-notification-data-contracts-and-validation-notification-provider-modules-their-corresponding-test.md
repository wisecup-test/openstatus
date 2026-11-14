---
version: 2
name: 741a22b2-3034-445f-b770-035a98c1ec42-adoption-of-openstatus-db-src-schema-for-notification-data-contracts-and-validation-notification-provider-modules-their-corresponding-test
description: "Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Provider Modules Their Corresponding Test"
metadata:
  actual:
    adr_id: 741a22b2-3034-445f-b770-035a98c1ec42
    status: accepted
    candidate_group_id: 03a41a76-4059-4f8c-9e4a-e5b1302235a8
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Notification Data Contracts and Validation: Notification Provider Modules Their Corresponding Test

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/discord`

Applies to:
- `src/**`

## Context

- Notification dispatchers receive serialized payload data requiring strict runtime integrity checks before propagating to third-party endpoints.
- Decoupled notification subpackages require a synchronized contract definition to prevent schema drift between persistent storage representations and delivery payloads.
- Test mock utilities require identical contract validation logic to simulate real-world notification dispatch behavior accurately.

## Problem Statement

External notification dispatchers handle serialized event payloads that risk silent failure or invalid network transmissions if field structures diverge from domain models. Without centralized contract enforcement, notification handlers and test mocks risk duplicating schema definitions and bypassing runtime data validation.

## Decision

1. MUST: Notification provider modules and their corresponding test harnesses MUST import and validate domain payload structures using schema definitions provided by @openstatus/db/src/schema.

## Policy Block

- MUST Notification provider modules and their corresponding test harnesses MUST import and validate domain payload structures using schema definitions provided by @openstatus/db/src/schema.

In scope:
- Notification provider integration modules handling inbound event payloads
- Test mocks and verification suites simulating notification data dispatch

Out of scope:
- Internal presentation components that do not interact with serialized notification event models
- Stateless utility helpers that operate on primitive values without domain entity dependencies

Exceptions:
- EXC-20-001: External webhook payloads originating from third-party vendor callbacks that do not map to internal domain entity schemas

## Rationale

- Static analysis of notification and mock implementations demonstrates centralized reliance on @openstatus/db/src/schema for parsing notification data structures prior to network invocation.
- Reusing core database schema contracts for notification payloads ensures compile-time and runtime consistency across data generation and notification consumption boundaries.
- Parsing serialized payload data through unified schema definitions prevents unhandled runtime exceptions during external client transmissions.

## Consequences

Positive:
- Guarantees runtime schema validation for notification payloads across both production handlers and test mock suites.
- Eliminates redundant contract definitions by establishing a single source of truth for domain data structures.
- Prevents malformed external network requests through early validation failure handling.

Negative:
- Couples notification provider modules directly to the shared database schema module, requiring updates when schema definitions change.
- Introduces runtime parsing overhead upon deserializing notification payload strings.

## Alternatives

- Decoupled Local Schema Definitions per Notification Provider (rejected)
  Rejected because: Creating isolated schemas within individual provider packages increases maintenance overhead and leads to contract drift between database entities and notification payloads.
  When valid: Valid when notification providers ingest third-party webhooks with vendor-controlled payloads independent of internal domain entities.
- Unvalidated Raw Ingestion with Compile-Time Type Assertions (rejected)
  Rejected because: Compile-time type assertions provide no runtime safety, risking downstream failures when serialized data payloads diverge from expected structures.
  When valid: Valid in high-throughput telemetry pipelines where schema validation creates measurable performance bottlenecks and payloads are pre-validated upstream.

## Risks

- Breaking changes in the shared schema package could cascade into notification delivery failures if versions become out of sync.
  Mitigation: Enforce workspace-wide contract testing and automated test execution across notification packages whenever shared schemas change.
  Owner: Platform Engineering
- Excessive payload parsing latency could degrade dispatch throughput during notification spikes.
  Mitigation: Profile schema parsing operations and ensure payloads contain only the necessary fields required for notification dispatch.
  Owner: Notification Service Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure notification payload deserialization to parse raw notification data strings through the designated schema parser before constructing external client request objects.
- Ensure mock data generators in test suites invoke the same shared schema parser to maintain contract equivalence with production event consumers.

## Continuation Context


Verify commands:
- Discover the workspace dependency resolution artifact and verify all internal schema package references resolve consistently.
- Discover and execute the project verification script for static type checking across notification packages.
- Discover and execute the project test runner to validate notification dispatch and mock test suites against shared schema contracts.

Accept when:
- All notification provider implementations parse event payload data through schema validators imported from the shared schema module without type or runtime errors.
- Test mock suites execute successfully and verify payload compliance using identical shared schema definitions.
- Static type analysis and automated test suites across notification packages pass cleanly without unhandled validation rejections.

## Enforcement

- Verified by: Automated continuous integration pipelines executing workspace-wide type checks and test suites.
- Verified by: Peer code reviews verifying that new notification providers consume shared schema contracts rather than defining custom entity models.
- Violation handling: Pull requests introducing unvalidated notification payload ingestion or duplicated schema definitions will fail automated checks and be blocked from merge.
- Violation handling: Engineers must update non-compliant modules to import and validate via the designated shared schema module.
- Exception process: Submit an architectural exception request detailing why the external payload cannot be represented by the shared schema module, requiring approval from the platform lead.