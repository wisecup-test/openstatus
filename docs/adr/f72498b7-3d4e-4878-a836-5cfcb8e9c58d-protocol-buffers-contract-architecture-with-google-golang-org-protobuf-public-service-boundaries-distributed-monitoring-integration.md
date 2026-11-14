---
version: 2
name: f72498b7-3d4e-4878-a836-5cfcb8e9c58d-protocol-buffers-contract-architecture-with-google-golang-org-protobuf-public-service-boundaries-distributed-monitoring-integration
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Public Service Boundaries Distributed Monitoring Integration"
metadata:
  actual:
    adr_id: f72498b7-3d4e-4878-a836-5cfcb8e9c58d
    status: accepted
    candidate_group_id: 64d684e7-b04a-42f9-82f0-1a520177c4fd
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/**
      - handlers/**
      - pkg/**
      - proto/private_location/v1/**
      - request/**
---

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Public Service Boundaries Distributed Monitoring Integration

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/**`
- `handlers/**`
- `pkg/**`
- `proto/private_location/v1/**`
- `request/**`

## Context

- Distributed monitoring runners and coordinator services require structured, backward-compatible data exchanges for region metrics, health checks, and execution assertions.
- Unstructured transport formats introduce schema drift and unverified runtime type conversions across remote service boundaries.
- The codebase establishes formal contract definitions using google.golang.org/protobuf to enforce interface compatibility across independent execution processes.

## Problem Statement

Public and distributed service interactions between coordinator services, external clients, and monitoring runners require deterministic data schemas and backward-compatible serialization. Without explicit interface contracts and strict message type enforcement, distributed components experience schema divergence, deserialization failures, and unvalidated payloads.

## Decision

1. MUST: Public service boundaries and distributed monitoring integration contracts MUST be defined using Protocol Buffers schemas implemented through google.golang.org/protobuf message types.

## Policy Block

- MUST Public service boundaries and distributed monitoring integration contracts MUST be defined using Protocol Buffers schemas implemented through google.golang.org/protobuf message types.

In scope:
- Public and external API boundaries defining monitoring jobs, regional health telemetry, and service contracts
- Cross-process communication between coordinator services and distributed checker execution daemons

Out of scope:
- Internal single-process utility functions that do not cross service boundaries
- Unstructured local diagnostic logs and operational health ping responses

Exceptions:
- EXC-25-001: Simple unauthenticated operational health endpoints return minimal diagnostic strings

## Rationale

- Static schema generation guarantees field-level backward compatibility and explicit contract evolution across distributed nodes.
- Compiled contract interfaces eliminate manual serialization parsing and runtime type assertion errors in regional job runners.
- Unified contract definitions provide deterministic descriptor reflection for telemetry mapping and cross-boundary metric collection.

## Consequences

Positive:
- Strict type safety and validation at service interface boundaries
- Deterministic binary serialization with established backward-compatibility guarantees
- Decoupling of transport serialization mechanics from core job execution logic

Negative:
- Requirement to regenerate and synchronize message contract code across consumer packages whenever schemas change
- Increased architectural overhead for simple data transfers that require formal contract definitions

## Alternatives

- Ad-hoc JSON schemas without formal contract definitions (rejected)
  Rejected because: Lacks binary serialization efficiency, explicit type reflection, and compile-time contract validation
  When valid: Simple external REST endpoints consumed exclusively by third-party browser clients
- Hand-written serialization interfaces and manual type decoders (rejected)
  Rejected because: Substantially increases boilerplate maintenance and risk of schema divergence across distributed processes
  When valid: Single-binary embedded environments without distributed service boundaries

## Risks

- Schema drift if generated contract structures are modified independently across repositories or packages
  Mitigation: Enforce automated interface compatibility checks and centralized schema generation during build validation
  Owner: platform engineering team
- Runtime panics if nil message pointers are accessed without checking ProtoMessage descriptor validity
  Mitigation: Apply defensive assertion comparators and interface validation prior to accessing nested message fields
  Owner: backend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Generate contract structures into designated service contract packages and import them into job runner and scheduler modules.
- Implement client network interceptors to uniformly attach authentication tokens and context deadlines across external client calls.

## Continuation Context


Verify commands:
- echo "Discover and execute repository test suites governing external API contracts"
- echo "Discover and execute repository schema linters and contract compatibility checks"
- echo "Verify lock artifacts to ensure resolved dependency versions match manifest constraints"

Accept when:
- All contract validation tests and protocol buffer message serialization test suites execute successfully without errors.
- Repository dependency verification confirms that the resolved contract library matches the locked artifact version.
- All external service communications and message structures strictly implement required protocol message and reflection interfaces.

## Enforcement

- Verified by: Continuous integration contract test pipelines
- Verified by: Pull request code review verifying adherence to protocol buffer interfaces
- Violation handling: Automated build failure on schema divergence or unvalidated message structures
- Violation handling: Rejection of pull requests introducing untyped external API contracts
- Exception process: Submit an architectural exception request detailing why a formal protocol buffer schema cannot be utilized for the specified boundary, requiring approval from the architecture review committee.