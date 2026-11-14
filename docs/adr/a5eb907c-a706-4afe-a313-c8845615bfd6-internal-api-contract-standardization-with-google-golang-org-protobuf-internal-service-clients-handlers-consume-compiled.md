---
version: 2
name: a5eb907c-a706-4afe-a313-c8845615bfd6-internal-api-contract-standardization-with-google-golang-org-protobuf-internal-service-clients-handlers-consume-compiled
description: "Internal API Contract Standardization with google.golang.org/protobuf: Internal Service Clients Handlers Consume Compiled"
metadata:
  actual:
    adr_id: a5eb907c-a706-4afe-a313-c8845615bfd6
    status: accepted
    candidate_group_id: 523c2937-cde9-4774-8810-baf86298bb1a
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
      - proto/private_location/v1/**
---

# Internal API Contract Standardization with google.golang.org/protobuf: Internal Service Clients Handlers Consume Compiled

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`
- `internal/tinybird/**`
- `internal/workflows/**`
- `proto/private_location/v1/**`

## Context

- Internal service boundaries for private location monitoring require strongly-typed, cross-component communication contracts for HTTP, DNS, TCP, and telemetry assertion models.
- Decoupled subsystems require uniform schema definitions to prevent contract drift and serialization mismatches between internal API clients and servers.
- The project uses google.golang.org/protobuf to define, reflect, and serialize internal schema specifications.

## Problem Statement

Distributed private location subsystems require robust, unambiguous API contracts for exchanging complex monitor definitions, telemetry assertions, and execution results. Unstructured or unverified payload serialization leads to schema drift, runtime unmarshaling exceptions, and broken boundary integrations across internal services.

## Decision

1. SHOULD: Internal service clients and handlers SHOULD consume compiled message descriptor data through thread-safe, one-time initialization rather than per-request descriptor parsing.

## Policy Block

- SHOULD Internal service clients and handlers SHOULD consume compiled message descriptor data through thread-safe, one-time initialization rather than per-request descriptor parsing.

In scope:
- All internal service contracts, message schemas, and telemetry models exchanged across private location service boundaries.
- All internal monitor configuration specifications, execution requests, and assertion payloads.

Out of scope:
- Unmanaged external third-party integration webhooks where payloads are strictly determined by third-party vendor specifications.
- Static system health and readiness probe endpoints using plain status responses.

## Rationale

- Adopting google.golang.org/protobuf establishes compile-time schema validation and strict boundary contracts across internal monitoring components.
- Implementing standardized reflection and message interfaces ensures consistent serialization, descriptor access, and compatibility checking without runtime drift.
- Compiled message structs provide superior serialization efficiency and deterministic backward compatibility compared to manual schema mapping.

## Consequences

Positive:
- Guarantees deterministic, backward-compatible binary serialization across distributed internal service boundaries.
- Provides type safety and contract compilation validation through ProtoMessage and ProtoReflect interfaces.
- Standardizes internal monitoring specifications, telemetry definitions, and assertion models on a uniform protocol framework.

Negative:
- Requires code generation steps before schema updates are accessible to downstream Go modules.
- Introduces reflection and descriptor overhead during initial descriptor compression and lookup.
- Schema updates require coordinated contract maintenance across internal API producers and consumers.

## Alternatives

- Hand-crafted JSON serialization over internal HTTP endpoints (rejected)
  Rejected because: Lacks deterministic schema compilation, reflection guarantees, and type-safe schema evolution, leading to silent payload drift between services.
  When valid: Valid only for public facing webhooks or unmanaged third-party webhook integrations requiring arbitrary payload flexibility.
- Dynamic map-based unstructured payloads (rejected)
  Rejected because: Eliminates static contract validation, prevents compile-time reflection, and increases runtime decoding error rates across distributed service boundaries.
  When valid: Valid during rapid prototyping phases where data schemas are undefined and transient.

## Risks

- Breaking changes introduced through backward-incompatible schema updates in internal message definitions.
  Mitigation: Maintain backward compatibility rules ensuring new fields use optional designations and existing field numbers are never reassigned.
  Owner: engineering team
- Serialization and reflection performance overhead during high-frequency payload transformations.
  Mitigation: Use compiled accessor methods and avoid runtime descriptor reflection in high-throughput hot paths.
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
- Generated message structs must encapsulate descriptor registration and avoid exposing mutable descriptor states directly to application consumers.
- Internal client integrations must rely on compiled accessors rather than reflection-based inspection in performance-critical execution paths.

## Continuation Context


Verify commands:
- Discover and execute the project contract linting and verification scripts to confirm internal schema compliance.
- Discover and execute the project test suite covering internal API serialization and message boundary validation.

Accept when:
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

## Enforcement

- Verified by: Static analysis checks and schema linters in continuous integration pipelines.
- Verified by: Automated boundary test suites verifying ProtoMessage and ProtoReflect contract conformance.
- Verified by: Architecture peer reviews for any modification to internal communication boundaries.
- Violation handling: Pull requests introducing uncompiled or non-conforming internal API payloads are blocked by automated continuous integration gates.
- Violation handling: Violations identified during code review require refactoring to use compiled message definitions prior to merging.
- Exception process: Exceptions require approval from the lead systems architect and must include a migration strategy toward compiled schemas.
- Exception process: Temporary exceptions must define manual schema validation wrappers and automated serialization regression tests.