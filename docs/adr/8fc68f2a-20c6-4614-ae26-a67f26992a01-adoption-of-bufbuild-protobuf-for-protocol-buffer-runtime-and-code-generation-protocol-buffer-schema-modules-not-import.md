---
version: 2
name: 8fc68f2a-20c6-4614-ae26-a67f26992a01-adoption-of-bufbuild-protobuf-for-protocol-buffer-runtime-and-code-generation-protocol-buffer-schema-modules-not-import
description: "Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Protocol Buffer Schema Modules Not Import"
metadata:
  actual:
    adr_id: 8fc68f2a-20c6-4614-ae26-a67f26992a01
    status: accepted
    candidate_group_id: 09c3ec39-9b4e-4ca5-a0fd-52f7a3257726
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/**
---

# Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Protocol Buffer Schema Modules Not Import

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/proto`

Applies to:
- `gen/ts/**`

## Context

- Generated protocol buffer modules require a consistent, type-safe runtime library to support serialization, deserialization, and schema validation.
- Multiple schema definitions across the repository import code generation runtime primitives and standard well-known types from a unified library ecosystem.
- Standardizing on a single protocol buffer runtime library prevents fragmentation and version incompatibility across modular packages.

## Problem Statement

Protocol buffer definitions across the repository require a dependable, standardized runtime library for binary message serialization, reflection, and well-known type management. Without an enforced runtime standard, disparate packages risk introducing conflicting serialization libraries, leading to runtime incompatibilities, redundant bundle overhead, and divergent type contracts across module boundaries.

## Decision

1. MUST_NOT: Protocol buffer schema modules MUST NOT import alternative or conflicting protocol buffer runtime libraries within workspace packages.

## Policy Block

- MUST_NOT Protocol buffer schema modules MUST NOT import alternative or conflicting protocol buffer runtime libraries within workspace packages.

In scope:
- All packages and modules containing generated protocol buffer definitions or consuming protocol buffer serialization logic.
- All TypeScript message definitions, service descriptors, and schema annotations within the repository.

Out of scope:
- Packages or boundaries utilizing alternative serialization formats that do not interface with protocol buffers.
- External legacy integrations explicitly segregated from the shared protocol buffer compilation pipeline.

## Rationale

- IR evidence across multiple schema modules confirms standard reliance on @bufbuild/protobuf runtime modules, including codegen runtime foundations and well-known types.
- Consolidating on this specific runtime ensures binary compatibility, predictable serialization behavior, and shared type definitions across all protocol buffer schema consumers.
- Relying on standard well-known type modules eliminates redundant implementations and guarantees interoperability across protocol buffer messages.

## Consequences

Positive:
- Establishes a uniform, standardized TypeScript runtime API for protocol buffer message handling and schema extensions.
- Provides strongly typed well-known type representations without requiring external bespoke type definitions.
- Ensures consistent serialization, deserialization, and field validation behaviors across all protocol buffer schema modules.

Negative:
- Couples all generated protocol buffer artifacts and consuming services to the specific runtime semantics and release cadence of the adopted library.
- Requires schema regeneration and dependency alignment across consuming packages whenever upstream runtime interfaces introduce breaking changes.

## Alternatives

- Adopting legacy or unmaintained protocol buffer runtime bindings (rejected)
  Rejected because: Legacy implementations introduce heavier bundle footprints, lack modern TypeScript type generation structures, and do not provide streamlined well-known type integrations.
  When valid: Valid only when maintaining legacy environments constrained by deprecated runtime engines that cannot upgrade.
- Inlining custom binary serialization and deserialization routines (rejected)
  Rejected because: Handcrafted serialization is prone to specification drift, lacks interoperability validation, and increases long-term maintenance burden.
  When valid: Valid only for constrained micro-runtimes where full protocol buffer specifications cannot be executed.

## Risks

- Breaking interface changes between major releases of the runtime library may desynchronize code generators and runtime packages.
  Mitigation: Follow the Lock-Version Grounding procedure to pin and verify exact runtime dependencies before generating or compiling schema code.
  Owner: engineering team
- Version divergence between modular packages could result in duplicate runtime installations and type mismatch errors.
  Mitigation: Enforce automated dependency resolution checks in continuous integration to guarantee consistent package versions across all workspace boundaries.
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
- Inspect the root dependency manifest and lock artifact to determine the active protocol buffer runtime dependencies across all workspace modules.
- Ensure all generated protocol buffer artifacts are generated exclusively using the generator plugins matching the installed runtime library packages.
- Reference the official documentation for the exact resolved runtime library version to verify method signatures and type constructs.

## Continuation Context


Verify commands:
- Find the project dependency manifest and inspect the declared runtime dependencies to identify the protocol buffer library.
- Inspect the repository resolution lock artifact to verify the exact resolved version of the protocol buffer runtime.
- Run the project verification scripts to ensure all generated protocol buffer modules build cleanly without dependency or type errors.

Accept when:
- The package dependency manifest and lock artifact define the resolved protocol buffer runtime library without version discrepancies.
- Automated static verification confirms all generated protocol buffer modules resolve runtime primitives exclusively from the adopted library packages.
- Repository test and build suites execute successfully with all protocol buffer serialization and validation tests passing.

## Enforcement

- Verified by: Continuous integration build steps validating that all schema modules compile against the resolved library dependencies.
- Verified by: Automated linting rules that detect and prevent unauthorized imports from alternative protocol buffer runtimes.
- Verified by: Code review approvals confirming adherence to standardized runtime usage in newly generated schema packages.
- Violation handling: Build and test failures in continuous integration pipelines whenever non-compliant imports are identified.
- Violation handling: Rejection of pull requests introducing unapproved runtime serialization dependencies.
- Violation handling: Automated reporting of unauthorized runtime dependencies to package maintainers.
- Exception process: Submit an architectural exception request detailing technical incompatibility with the adopted protocol buffer runtime.
- Exception process: Require formal review and sign-off by repository maintainers prior to merging any code utilizing divergent serialization libraries.
- Exception process: Document remediation pathways and lifecycle migration targets for any approved temporary exceptions.