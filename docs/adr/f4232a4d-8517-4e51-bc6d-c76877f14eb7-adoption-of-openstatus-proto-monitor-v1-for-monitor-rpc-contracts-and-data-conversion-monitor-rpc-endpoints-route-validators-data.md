---
version: 2
name: f4232a4d-8517-4e51-bc6d-c76877f14eb7-adoption-of-openstatus-proto-monitor-v1-for-monitor-rpc-contracts-and-data-conversion-monitor-rpc-endpoints-route-validators-data
description: "Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Monitor Rpc Endpoints Route Validators Data"
metadata:
  actual:
    adr_id: f4232a4d-8517-4e51-bc6d-c76877f14eb7
    status: accepted
    candidate_group_id: e7f271a5-fe3d-4441-b875-80268f958fce
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @openstatus/proto/monitor/v1 for Monitor RPC Contracts and Data Conversion: Monitor Rpc Endpoints Route Validators Data

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/rpc/**`

## Context

- RPC route handlers and data access layers require consistent, strongly typed message specifications across service boundaries.
- Monitor domain logic encompasses multiple interdependent data representations, including assertions, enumerations, regions, headers, and response logs.
- Disconnected schema definitions across network endpoints and internal domain models introduce the risk of data drift and serialization incompatibilities.
- The service architecture isolates interface definitions within a dedicated module to standardize request validation, conversion, and transport schemas across monitor endpoints.

## Problem Statement

RPC endpoint handlers, request validators, and data converters require a unified, type-safe contract definition to prevent serialization drift and schema discrepancies between transport payloads and internal domain representations.

## Decision

1. MUST: All monitor RPC endpoints, route validators, and data converters MUST adopt @openstatus/proto/monitor/v1 message definitions as their canonical contract interface.

## Policy Block

- MUST All monitor RPC endpoints, route validators, and data converters MUST adopt @openstatus/proto/monitor/v1 message definitions as their canonical contract interface.

In scope:
- RPC route definitions, request validation routines, and data converters handling monitor domain operations.
- Module boundaries adapting between network transport serialization and internal domain models.

Out of scope:
- Internal database persistence models and schema tables that do not cross RPC service boundaries.
- RPC endpoints and routing modules belonging to other service domains.

## Rationale

- Relying on @openstatus/proto/monitor/v1 creates a single source of truth for monitor contracts across routing, validation, and converter layers.
- Encapsulating message schemas within a dedicated contract module decouples protocol evolution from internal database schema migrations.
- Structured conversion between domain entities and protocol buffer message types prevents unintended schema leakage across network boundaries.

## Consequences

Positive:
- Eliminates schema drift across monitor endpoint handlers and data conversion pipelines.
- Enforces strong typing and consistent serialization formats across RPC service boundaries.
- Facilitates independent schema evolution and validation auditing across monitor features.

Negative:
- Requires explicit mapping layers between internal database representations and protocol message structures.
- Adds maintenance overhead when coordinating schema updates across contract definitions and domain converters.

## Alternatives

- Direct exposure of database schema types over RPC endpoints (rejected)
  Rejected because: Couples internal storage layout directly to network interfaces, introducing security risks and preventing independent schema evolution.
  When valid: Valid only in rapid prototypes with no external consumers or API stability guarantees.
- Inline schema declarations within individual route handlers (rejected)
  Rejected because: Duplicating schema structures across handlers leads to specification divergence, validation discrepancies, and maintenance friction.
  When valid: Valid only in single-endpoint exploratory utilities with no shared data models.

## Risks

- Conversion boilerplate between internal persistence models and contract types may diverge if schema updates omit converter adjustments.
  Mitigation: Enforce comprehensive static type verification and automated converter test suites across contract boundaries.
  Owner: Backend Engineering Team
- Stale dependency resolution could introduce schema mismatches if contract versions are not synchronized across environments.
  Mitigation: Verify contract dependency declarations against project resolution artifacts during continuous integration pipelines.
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
- Isolate converter logic within dedicated modules responsible for bidirectional translation between internal domain models and contract types.
- Perform all input schema validation at the RPC entry point prior to invoking converter functions or service business logic.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type verification script to ensure all monitor RPC handlers conform to contract signatures.
- Discover and run the test suite covering data converters and RPC route handlers to validate message serialization and parsing.

Accept when:
- Static type analysis passes with zero type mismatch errors across all monitor RPC handlers and converters.
- All unit and integration tests for monitor converters and route validators execute successfully.

## Enforcement

- Verified by: Automated static type checking and test execution in continuous integration pipelines.
- Verified by: Peer code review verification of converter and handler imports for contract compliance.
- Violation handling: Pull requests introducing direct persistence model leaks or bypassing contract schemas are rejected at code review.
- Violation handling: Continuous integration builds fail upon detecting contract type violations.
- Exception process: Exceptions require documented architectural justification and written sign-off from the backend engineering lead.