---
version: 2
name: 80afdd1e-d1aa-48f5-99ad-95be123f873d-primary-datastore-access-delegation-via-openstatus-services-procedures-that-consume-list-operations-openstatus
description: "Primary Datastore Access Delegation via @openstatus/services: Procedures That Consume List Operations Openstatus"
metadata:
  actual:
    adr_id: 80afdd1e-d1aa-48f5-99ad-95be123f873d
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Procedures That Consume List Operations Openstatus

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`

## Context

- API procedures handle requests from multiple client surfaces requiring consistent persistence operations across diverse business domains.
- Direct database queries within transport route handlers couple routing protocols to schema implementations and duplicate tenant isolation logic.
- The codebase establishes dedicated domain packages under @openstatus/services to encapsulate persistence queries, transaction boundaries, and domain filtering.
- Adapting procedure contexts to service contexts through a shared adapter standardizes authentication, authorization, and workspace tenant isolation.

## Problem Statement

Coupling primary datastore operations directly to API route definitions leads to fragmented tenant boundary enforcement, duplicated query logic, inconsistent error translation across communication protocols, and increased risk of unbounded datastore queries.

## Decision

1. MUST: API procedures that consume list operations from @openstatus/services MUST explicitly provide pagination constraints or bounded sentinel limits to prevent unbounded datastore scans.

## Policy Block

- MUST API procedures that consume list operations from @openstatus/services MUST explicitly provide pagination constraints or bounded sentinel limits to prevent unbounded datastore scans.

In scope:
- All API router procedures performing read or write operations against primary datastores.
- Protected and public procedures within the API package boundary.

Out of scope:
- Specialized public aggregation endpoints where high-cardinality multi-source streaming or external analytics fallbacks require composite query orchestrations.
- In-memory operations or local ephemeral token utilities not backed by primary datastore records.

Exceptions:
- EXC-28-001: A public status rendering route requires composite transactional joins across multiple relational hierarchies with performance constraints incompatible with isolated domain service calls.

## Rationale

- Centralizing primary datastore queries within @openstatus/services establishes an explicit abstraction barrier between transport protocols and storage schemas.
- Encapsulating persistence operations enables reuse of query logic and domain business rules across multiple consumer surfaces without schema leakage.
- Context transformation via toServiceCtx ensures tenant identity and workspace permissions are verified uniformly before executing datastore queries.
- Standardized error handling via toTRPCError guarantees that persistence failures and domain invariants map cleanly to predictable client error codes.

## Consequences

Positive:
- Enforces strict separation between transport protocol handling and primary datastore operations.
- Guarantees uniform multi-tenant workspace isolation across all domain queries through standardized context propagation.
- Centralizes error mapping, data filtering, and sentinel pagination bounds across diverse API surfaces.

Negative:
- Introduces an additional layer of indirection requiring explicit context conversion for all datastore operations.
- Requires coordinate updates across both domain service packages and API routing modules when changing datastore query contracts.

## Alternatives

- Direct datastore client queries embedded within route handlers (rejected)
  Rejected because: Scatters datastore schema access across routing endpoints, duplicates tenant filtering, and tightly couples transport protocols to datastore implementations.
  When valid: Small monolithic prototypes with a single transport protocol and flat permission structures.
- Global active-record models invoked directly from routes (rejected)
  Rejected because: Lacks explicit tenant context propagation and makes transaction boundaries difficult to manage across modular packages.
  When valid: Applications with single-tenant architecture and simple create-read-update-delete workflows.

## Risks

- Service layer queries returning unpaginated collections could overload memory or datastore connections.
  Mitigation: Enforce explicit limit and offset parameters or bounded sentinel ceilings in all list queries.
  Owner: API Platform Engineering
- Context conversion failures between transport and service layers could lead to authorization bypass.
  Mitigation: Validate that toServiceCtx strictly checks required tenant session fields before constructing service context.
  Owner: Security and Platform Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct all service queries by mapping verified procedure input schemas to domain service parameter structures.
- Ensure all service function calls within router handlers are wrapped in structured exception blocks delegating error conversion to standard mapping utilities.

## Continuation Context


Verify commands:
- Discover the repository build and type-checking scripts from the project configuration and execute them to verify that all API procedures correctly adhere to service function signatures.
- Discover and run the test suite scripts declared in the workspace configuration to validate that router endpoints correctly adapt context and return expected domain results.

Accept when:
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

## Enforcement

- Verified by: Continuous integration type checks verifying service interface compliance.
- Verified by: Static lint rules flagging direct datastore imports within API router modules.
- Verified by: Peer code review for all new and modified API router procedures.
- Violation handling: Pull requests containing direct datastore queries in router modules are blocked until refactored into service calls.
- Violation handling: Build failures triggered on unhandled service exceptions or malformed context adaptation.
- Exception process: Submit an architectural review request detailing performance benchmarks and query requirements justifying bypass of domain services.
- Exception process: Obtain explicit sign-off from API and data architecture owners prior to merging direct query overrides.