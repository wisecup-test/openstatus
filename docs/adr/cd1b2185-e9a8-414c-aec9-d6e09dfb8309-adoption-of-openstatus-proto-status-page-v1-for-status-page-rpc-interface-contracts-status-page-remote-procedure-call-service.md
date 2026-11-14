---
version: 2
name: cd1b2185-e9a8-414c-aec9-d6e09dfb8309-adoption-of-openstatus-proto-status-page-v1-for-status-page-rpc-interface-contracts-status-page-remote-procedure-call-service
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Status Page Remote Procedure Call Service"
metadata:
  actual:
    adr_id: cd1b2185-e9a8-414c-aec9-d6e09dfb8309
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Status Page Remote Procedure Call Service

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/rpc/handlers/status-page/**`

## Context

- The status page subsystem exposes remote procedure call endpoints that require strongly typed request and response contracts across service boundaries.
- Status page endpoint handlers and data conversion modules require shared interface definitions to ensure consistent wire serialization and deserialization.
- Direct exposure of database schema models across network interfaces couples persistence models with client-facing contracts and degrades boundary encapsulation.

## Problem Statement

Without standardized contract definitions, remote procedure call handlers risk tightly coupling service transport interfaces to internal database persistence schemas or introducing inconsistent payload structures across status page endpoints.

## Decision

1. MUST: Status page remote procedure call service handlers and conversion layers MUST use @openstatus/proto/status_page/v1 contracts for all boundary payload definitions and wire message serialization.

## Policy Block

- MUST Status page remote procedure call service handlers and conversion layers MUST use @openstatus/proto/status_page/v1 contracts for all boundary payload definitions and wire message serialization.

In scope:
- Remote procedure call handlers responsible for status page domain operations.
- Data conversion modules transforming internal persistence entities to and from status page transport contracts.

Out of scope:
- Internal database migrations and table definition schemas that do not cross service boundary interfaces.
- Client-side state management stores and UI rendering components.

## Rationale

- Adopting @openstatus/proto/status_page/v1 establishes a single source of truth for status page remote procedure call contracts, ensuring consistent wire formats across endpoint handlers.
- Decoupling service contract definitions from internal database schema entities allows independent evolution of persistence models without breaking external API contracts.
- Dedicated converter modules operating against @openstatus/proto/status_page/v1 provide explicit mapping boundaries and centralized type transformations.

## Consequences

Positive:
- Provides strong type safety and explicit contract boundaries across all status page remote procedure call endpoints.
- Prevents accidental leakage of sensitive internal database schema fields into external transport payloads.
- Facilitates backward-compatible schema evolution by decoupling database persistence from transport contract schemas.

Negative:
- Requires maintaining dedicated converter functions between persistence models and protocol contracts.
- Introduces overhead when adding new domain fields because changes must be reflected in both transport contracts and conversion logic.

## Alternatives

- Expose internal database persistence schemas directly over remote procedure call interfaces (rejected)
  Rejected because: Directly exposing persistence entities tightly couples database storage structure to external client contracts and risks exposing sensitive internal fields.
  When valid: Only viable in rapid prototyping or purely internal monolithic utilities where wire contracts and storage models are identical.
- Ad-hoc manual type assertions and untyped JSON payload serialization (rejected)
  Rejected because: Lacks compile-time contract verification, leading to runtime serialization failures and contract drift between handlers and clients.
  When valid: Acceptable only in unstructured logging pipelines or arbitrary metadata ingestion endpoints.

## Risks

- Contract drift between internal database schema updates and protocol contract transformations.
  Mitigation: Implement strict conversion layer unit tests and automated contract verification across endpoints.
  Owner: Backend Engineering Team
- Performance overhead from redundant object transformations between database records and protocol message structures.
  Mitigation: Benchmark converter functions and keep mapping logic purely functional without intermediate deep copies.
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
- Structure data transformation functions in dedicated converter modules that map between internal persistence types and @openstatus/proto/status_page/v1 contracts.
- Ensure all remote procedure call handlers import request and response types exclusively from the contract module rather than reconstructing structural equivalents locally.

## Continuation Context


Verify commands:
- Discover the repository test runner from the dependency manifest and execute the test suite covering status page remote procedure call handlers and converters.
- Discover the project static type checker and run type analysis to ensure strict adherence to protocol contract interfaces.
- Discover the repository linting and formatting scripts and execute them across the codebase to verify architectural boundary conformance.

Accept when:
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

## Enforcement

- Verified by: Automated continuous integration pipelines executing type checking and test verification suites.
- Verified by: Peer code reviews validating that remote procedure call endpoints exclusively expose contract types from @openstatus/proto/status_page/v1.
- Verified by: Static analysis checking for unauthorized imports or direct database schema exposure across route handlers.
- Violation handling: Pull requests introducing non-contract types or direct database schema leaks into remote procedure call responses will be blocked at continuous integration.
- Violation handling: Identified contract violations must be refactored into dedicated converter mappings before merging.
- Exception process: Exceptions require written technical rationale and formal sign-off from the technical lead or principal architecture reviewer.
- Exception process: Approved exceptions must be tracked with remediation milestones to restore standard contract conformance.