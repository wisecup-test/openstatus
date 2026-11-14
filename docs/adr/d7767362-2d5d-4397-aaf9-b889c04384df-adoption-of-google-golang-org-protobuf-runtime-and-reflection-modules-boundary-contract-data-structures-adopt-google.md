---
version: 2
name: d7767362-2d5d-4397-aaf9-b889c04384df-adoption-of-google-golang-org-protobuf-runtime-and-reflection-modules-boundary-contract-data-structures-adopt-google
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Boundary Contract Data Structures Adopt Google"
metadata:
  actual:
    adr_id: d7767362-2d5d-4397-aaf9-b889c04384df
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Boundary Contract Data Structures Adopt Google

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `proto/private_location/v1/**`

## Context

- Service boundaries require standardized, strongly typed contract definitions to ensure interoperability across internal components.
- Distributed message exchange demands efficient binary serialization and introspection capabilities that remain backward and forward compatible.
- Multiple boundary modules incorporate runtime metadata descriptors and reflection interfaces to inspect message contracts programmatically.

## Problem Statement

Distributed system components require a consistent, type-safe contract mechanism that enables cross-boundary serialization and programmatic schema inspection without relying on unversioned or loosely typed message formats. Without a unified reflection and runtime serialization framework, boundary contracts risk schema divergence, inconsistent serialization overhead, and lack of programmatic descriptor access.

## Decision

1. MUST: All boundary contract data structures MUST adopt google.golang.org/protobuf runtime and reflection modules to define message schemas, implement ProtoMessage, and provide ProtoReflect contract interfaces.

## Policy Block

- MUST All boundary contract data structures MUST adopt google.golang.org/protobuf runtime and reflection modules to define message schemas, implement ProtoMessage, and provide ProtoReflect contract interfaces.

In scope:
- Service boundary message definitions and inter-service contract structures
- Distributed telemetry, monitoring definitions, and contract serialization layers

Out of scope:
- Internal application domain models that do not cross service boundaries
- Ephemeral in-memory data structures strictly internal to local component execution

## Rationale

- Evidence demonstrates consistent implementation of ProtoMessage, ProtoReflect, and protoimpl runtime bindings across service boundary modules.
- Adopting google.golang.org/protobuf provides robust schema introspection, strong typing, and deterministic serialization performance across distributed environments.
- Lazily initialized descriptor data ensures thread safety while minimizing startup memory footprint.

## Consequences

Positive:
- Standardized binary serialization format ensures wire compatibility and type safety across service boundaries.
- Native reflection capabilities enable generic middleware, dynamic message inspection, and telemetry validation.
- Thread-safe lazy descriptor initialization prevents duplicate runtime decompression overhead.

Negative:
- Message models require maintenance of schema definitions and code generation lifecycles.
- Runtime reflection and descriptor data introduce baseline binary and memory footprint overhead.
- Schema changes require careful field numbering governance to avoid serialization incompatibilities.

## Alternatives

- Dynamic schema-less text-based serialization formats (rejected)
  Rejected because: Schema-less serialization introduces higher payload parsing overhead, runtime type ambiguity, and lacks built-in descriptor reflection.
  When valid: When payload readability and ad-hoc client querying take precedence over strict type contracts and wire efficiency.
- Custom binary serialization structs (rejected)
  Rejected because: Custom binary encodings require manual versioning, error-prone maintenance, and lack standardized descriptor introspection.
  When valid: When operating in hyper-constrained embedded hardware where third-party runtime overhead cannot be accommodated.

## Risks

- Drift between declared schema definitions and generated contract implementations
  Mitigation: Validate contract consistency within automated integration validation pipelines and enforce descriptor verification
  Owner: Engineering Team
- Improper field numbering or schema evolution causing wire serialization failures
  Mitigation: Enforce strict schema evolution rules prohibiting field tag renumbering or tag reuse
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the repository dependency manifest to verify the resolved library version and associated reflection runtime packages.
- Ensure all contract definitions implement ProtoReflect and Descriptor methods in accordance with the resolved runtime library documentation.
- Manage synchronization primitives carefully when registering or accessing raw descriptor data to ensure thread safety across concurrent requests.

## Continuation Context


Verify commands:
- Discover and execute the repository contract validation scripts to verify schema integrity and generated model alignment.
- Execute the repository test suite to validate serialization, deserialization, and reflection behavior across all boundary contracts.

Accept when:
- All contract validation scripts execute without reporting schema discrepancies or outdated contract structures.
- All boundary serialization and reflection test suites pass with zero errors.

## Enforcement

- Verified by: Automated contract consistency checks in continuous integration pipelines
- Verified by: Peer code reviews verifying adherence to ProtoMessage and ProtoReflect interface requirements
- Violation handling: Pull requests containing invalid contract implementations or bypassed serialization methods are blocked from merging
- Violation handling: Detected non-compliant contract implementations must be updated to implement required runtime interfaces
- Exception process: Architectural review and written approval from the lead architecture committee justifying alternative serialization protocols