---
version: 2
name: 21664dc8-0ac5-4528-886b-13da727b9cfc-adoption-of-google-golang-org-protobuf-for-structured-serialization-and-message-reflection-schemas-not-bypass-google-golang-org
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Schemas Not Bypass Google Golang Org"
metadata:
  actual:
    adr_id: 21664dc8-0ac5-4528-886b-13da727b9cfc
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Schemas Not Bypass Google Golang Org

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `proto/private_location/v1/**`

## Context

- The system requires consistent, strongly typed message contracts for defining monitoring configurations, assertion definitions, and telemetry data crossing service boundaries.
- Multiple subcomponents maintain structured schemas that require runtime reflection capabilities and uniform serialization behavior.
- Static analysis identifies consistent usage of google.golang.org/protobuf runtime and reflection modules across six schema definitions implementing standard contract interfaces.

## Problem Statement

Inter-service telemetry and monitoring definitions require strongly typed data structures with consistent binary serialization and runtime reflection. Without a standardized contract library, disparate components risk schema drift, incompatible wire formats, and duplicated serialization logic.

## Decision

1. MUST_NOT: Schemas MUST NOT bypass the google.golang.org/protobuf runtime reflection abstractions when inspecting message metadata across bounded contexts.

## Policy Block

- MUST_NOT Schemas MUST NOT bypass the google.golang.org/protobuf runtime reflection abstractions when inspecting message metadata across bounded contexts.

In scope:
- All data structures and schemas defining network protocols, monitor configurations, assertion schemas, and telemetry records crossing module or service boundaries.

Out of scope:
- Internal transient data structures and local in-memory variables that remain scoped within a single package without serialization requirements.

## Rationale

- The google.golang.org/protobuf library provides strongly typed message contracts with built-in reflection and descriptor management that prevent serialization incompatibilities.
- Implementing standard ProtoMessage and ProtoReflect interfaces enables generic message processing, descriptor inspection, and standardized wire-format serialization across monitoring modules.
- Observed patterns across six distinct schema files demonstrate an established architectural commitment to uniform protocol buffer runtime integration.

## Consequences

Positive:
- Enforces deterministic serialization and deserialization semantics across monitoring and telemetry schemas.
- Provides first-class runtime reflection and descriptor inspection via the ProtoReflect interface.
- Guarantees backward and forward compatibility mechanisms inherent to protocol buffer specifications.

Negative:
- Couples message definitions to runtime library abstractions and code-generation lifecycles.
- Introduces minor runtime initialization overhead during descriptor decompression and thread synchronization.
- Restricts schema structures to contract patterns supported by the reflection runtime.

## Alternatives

- Adopting standard library text-based serialization formats without dedicated contract schemas (rejected)
  Rejected because: Lacks built-in schema reflection interfaces, schema evolution rules, and efficient binary wire packing.
  When valid: Valid when human-readable debugging payloads are required for simple text configurations without schema evolution needs.
- Using manual binary encoding structures without reflection runtime interfaces (rejected)
  Rejected because: Significantly increases maintenance burden and risks field offset desynchronization across client boundaries.
  When valid: Valid in extreme low-overhead environments where external reflection runtime dependencies cannot be tolerated.

## Risks

- Schema definitions may drift if code generation is executed with incompatible runtime versions.
  Mitigation: Follow the mandatory lock-version grounding policy to verify runtime library versions against the repository lock artifact.
  Owner: Backend Engineering Team
- Concurrent access during raw descriptor decompression could introduce data races if not properly synchronized.
  Mitigation: Enforce thread-safe single-execution synchronization blocks for all raw descriptor decompression logic.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure generated message structures strictly adhere to the runtime interfaces ProtoMessage and ProtoReflect, delegating internal state tracking to the runtime implementation package.
- Coordinate descriptor registration and raw descriptor decompression through thread-safe single-execution synchronization blocks to guarantee concurrent safety across worker threads.

## Continuation Context


Verify commands:
- Find the repository build and validation script defined in the project configuration and execute it to verify message schema compilation.
- Locate the contract test execution command from the project manifest and run test suites validating ProtoMessage serialization and reflection behaviors.
- Query the repository dependency auditing mechanism to confirm all references to the serialization runtime resolve to the locked dependency version.

Accept when:
- All message definitions pass validation and schema generation checks without compilation errors.
- Test suites asserting message serialization, deserialization, and contract interface compliance pass completely.
- The resolved library version matches the exact specification recorded in the repository lock artifact.

## Enforcement

- Verified by: Continuous integration pipeline running automated schema validation and contract test suites.
- Verified by: Peer code review verifying interface conformance to ProtoMessage and ProtoReflect contracts.
- Violation handling: Pull requests introducing non-conforming serialization formats or missing contract interfaces are blocked from merging.
- Violation handling: Schema compilation failures immediately halt the build and deployment process.
- Exception process: Architectural exceptions require written submission and approval from the architecture review board documenting technical necessity.