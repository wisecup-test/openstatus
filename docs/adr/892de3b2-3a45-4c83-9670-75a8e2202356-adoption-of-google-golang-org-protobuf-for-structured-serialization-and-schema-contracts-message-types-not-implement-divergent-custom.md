---
version: 2
name: 892de3b2-3a45-4c83-9670-75a8e2202356-adoption-of-google-golang-org-protobuf-for-structured-serialization-and-schema-contracts-message-types-not-implement-divergent-custom
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Types Not Implement Divergent Custom"
metadata:
  actual:
    adr_id: 892de3b2-3a45-4c83-9670-75a8e2202356
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Types Not Implement Divergent Custom

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `proto/private_location/v1/**`

## Context

- Services require strongly typed, backward-compatible data interchange models to communicate monitoring specifications and telemetry assertions across boundaries.
- Manual serialization and untyped message formats introduce contract drift and parsing fragility across distributed checker components.
- Adopting a standardized protocol buffers runtime establishes uniform reflection capabilities and binary serialization efficiency across message definitions.

## Problem Statement

Distributed checker components exchange complex monitoring payloads and telemetry assertions that require strict schema contracts, backward compatibility, and minimal serialization overhead. Without a centralized, strongly typed serialization and reflection framework, service boundaries risk contract inconsistency, validation failures, and increased maintenance overhead.

## Decision

1. MUST_NOT: Message types MUST_NOT implement divergent custom binary serialization logic that bypasses the adopted protobuf runtime infrastructure.

## Policy Block

- MUST_NOT Message types MUST_NOT implement divergent custom binary serialization logic that bypasses the adopted protobuf runtime infrastructure.

In scope:
- Service boundary contracts, monitoring definitions, and telemetry data models requiring structured serialization.
- Components exchanging typed message payloads across checker subsystems.

Out of scope:
- Internal transient state management within isolated service components.
- Ad-hoc human-readable configuration and logging formats.

## Rationale

- Adopting google.golang.org/protobuf provides standard interfaces ProtoMessage and ProtoReflect, enabling type-safe serialization and reflection-based schema inspection.
- Runtime descriptor compression through protoimpl reduces binary footprint while preserving schema metadata necessary for dynamic payload decoding.
- Uniform adherence to standard contract methods eliminates proprietary marshaling routines and ensures cross-boundary compatibility.

## Consequences

Positive:
- Strict schema contracts eliminate serialization drift and contract mismatches across service boundaries.
- First-class reflection support enables dynamic message inspection and generic tooling integration.
- Optimized binary representation minimizes network payload size and parsing overhead.

Negative:
- Couples service contract definitions to the protobuf runtime and reflection library ecosystem.
- Runtime reflection and descriptor initialization introduce minor memory and CPU overhead at startup.

## Alternatives

- Adoption of text-based schemas for message serialization (rejected)
  Rejected because: Lacks native binary serialization efficiency, strict backward-compatibility guarantees, and unified reflection descriptors across language boundaries.
  When valid: When human-readable configuration files or browser-facing public endpoints require plain-text data interchange.
- Proprietary custom binary encoding routines (rejected)
  Rejected because: Increases maintenance burden, lacks reflection capabilities, and risks protocol errors during field additions.
  When valid: When extreme micro-benchmarked latency constraints strictly preclude any runtime reflection overhead.

## Risks

- Concurrent access during initial descriptor decompression could cause race conditions if initialization is not strictly synchronized.
  Mitigation: Enforce synchronized one-time initialization blocks for raw descriptor decompression across all message schemas.
  Owner: engineering team
- Version drift between runtime reflection libraries and compiled schema definitions could lead to runtime panics.
  Mitigation: Enforce lockfile verification and automated compatibility validation in continuous integration.
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
- Ensure all schema descriptors are registered through the runtime package to support reflection-based payload inspection and debugging.
- Verify that message lifecycle operations handle thread safety during concurrent descriptor access.

## Continuation Context


Verify commands:
- echo "Discover and execute project verification test suites from repository configuration"
- echo "Verify that project dependency lock records contain the resolved protobuf runtime dependency"

Accept when:
- All message definitions compile cleanly and satisfy ProtoMessage and ProtoReflect interfaces.
- Descriptor initialization executes successfully without concurrent race conditions in test suites.

## Enforcement

- Verified by: Automated static analysis and compilation checks in continuous integration pipelines.
- Verified by: Architecture review during pull requests modifying service contract definitions.
- Violation handling: Build rejection on schema contract or interface mismatch.
- Violation handling: Mandatory refactoring of non-compliant message implementations prior to merging.
- Exception process: Formal review and approval by architectural leads with documented justification in repository exception records.