---
version: 2
name: 24407c99-bb97-4110-995f-8b648b6f6ee9-adoption-of-google-golang-org-protobuf-for-structured-message-contracts-and-reflection-domain-components-not-bypass-protoreflect-descriptor
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Domain Components Not Bypass Protoreflect Descriptor"
metadata:
  actual:
    adr_id: 24407c99-bb97-4110-995f-8b648b6f6ee9
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Domain Components Not Bypass Protoreflect Descriptor

Status: proposed
Date: 2025-02-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `proto/private_location/v1/**`

## Context

- Domain monitoring specifications require structured, version-resilient contracts for assertions, location configurations, and monitor definitions.
- Multiple schema definitions across the codebase implement identical message contracts, reflection interfaces, and descriptor initialization logic.
- Boundary serialization requires strong schema validation and compact wire-format representations across client communication channels.

## Problem Statement

Exchanging complex monitoring specifications and location configurations across service boundaries without a unified serialization and reflection library risks contract drift, breaking wire changes, and inconsistent field serialization across services.

## Decision

1. MUST_NOT: Domain components MUST NOT bypass the protoreflect descriptor abstraction to manipulate internal serialized representations through raw memory offsets or direct descriptor mutation.

## Policy Block

- MUST_NOT Domain components MUST NOT bypass the protoreflect descriptor abstraction to manipulate internal serialized representations through raw memory offsets or direct descriptor mutation.

In scope:
- All structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

Out of scope:
- Internal non-serialized runtime helper data structures and transient local function state.

## Rationale

- Adopting google.golang.org/protobuf runtime and reflection modules establishes an immutable standard for message structure, introspection, and binary serialization.
- Evidence demonstrates consistent usage of protoimpl and protoreflect interfaces across all private location and monitoring contract definitions, proving a deliberate architectural convention.
- Standardizing on ProtoMessage and ProtoReflect contracts eliminates divergent serialization mechanisms and ensures cross-boundary protocol reliability.

## Consequences

Positive:
- Guarantees strict backward and forward schema compatibility across monitoring specifications and external boundary clients.
- Provides uniform introspective access and type-safe serialization contracts via ProtoReflect and ProtoMessage.
- Enforces thread-safe, single-pass descriptor initialization through synchronized runtime helper integration.

Negative:
- Introduces reliance on generated code artifacts and runtime reflection scaffolding that increases repository artifact footprint.
- Requires adherence to strict message descriptor initialization workflows and version-aligned runtime helper modules.

## Alternatives

- Custom reflection and hand-written struct serialization routines (rejected)
  Rejected because: Lacks standardized schema evolution, requires extensive manual serialization boilerplate, and increases risk of wire-format incompatibility.
  When valid: Valid only for ultra-minimal lightweight internal utilities where external dependencies cannot be linked.
- Dynamic map-based payload contracts without strict schema definitions (rejected)
  Rejected because: Prevents compile-time contract safety, impedes automated code generation, and obscures field semantics across client boundaries.
  When valid: Valid for completely unstructured dynamic payload pass-through where schemas are intentionally undefined.

## Risks

- Concurrent descriptor access causing initialization race conditions or memory faults.
  Mitigation: Enforce descriptor registration via synchronized single-execution blocks to ensure thread safety across concurrent message initializations.
  Owner: engineering team
- Runtime helper version mismatch between generated descriptors and the linked protobuf runtime library.
  Mitigation: Mandate dependency lock verification prior to code changes to ensure all runtime helper calls match the resolved library release.
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
- Ensure all message types are generated with proper synchronization primitives around descriptor data decompression to prevent multi-threaded data races.
- Maintain strict separation between generated protocol buffer contracts and domain logic by wrapping message instances with domain adapters where appropriate.

## Continuation Context


Verify commands:
- Discover and execute the repository contract validation test suite to confirm all generated boundary structures satisfy ProtoMessage and ProtoReflect interfaces.
- Execute the workspace static analysis checks to verify that descriptor initialization conforms to synchronized single-execution patterns without unauthorized memory manipulation.

Accept when:
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

## Enforcement

- Verified by: Automated repository verification checks enforcing interface compliance for all boundary payload structures.
- Verified by: Peer code review confirming all message schemas adhere to standard runtime and reflection conventions.
- Violation handling: Pull requests introducing unaligned serialization formats or bypassing protobuf reflection contracts are blocked in continuous integration.
- Violation handling: Non-compliant structures must be refactored to implement standard protobuf interfaces before deployment.
- Exception process: Submit an architectural variance request detailing why standard protobuf contracts cannot meet performance or protocol constraints.
- Exception process: Obtain sign-off from the technical lead before introducing alternative serialization mechanisms into boundary components.