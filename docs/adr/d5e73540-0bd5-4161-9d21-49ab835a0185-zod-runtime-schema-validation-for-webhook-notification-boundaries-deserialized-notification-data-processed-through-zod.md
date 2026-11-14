---
version: 2
name: d5e73540-0bd5-4161-9d21-49ab835a0185-zod-runtime-schema-validation-for-webhook-notification-boundaries-deserialized-notification-data-processed-through-zod
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: Deserialized Notification Data Processed Through Zod"
metadata:
  actual:
    adr_id: d5e73540-0bd5-4161-9d21-49ab835a0185
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: Deserialized Notification Data Processed Through Zod

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/webhook`

Applies to:
- `src/**`

## Context

- The notification subsystem processes serialized asynchronous event data and formats outbound HTTP webhook requests to external client endpoints.
- Payload data received from event notifications may contain malformed structures, missing attributes, or invalid state values.
- Outgoing webhook requests communicate monitor health transitions that external consumers depend on for automated operational actions.
- Relying solely on compile-time static type definitions leaves external network and serialization boundaries unprotected against runtime schema drift.

## Problem Statement

Without explicit runtime validation at service boundaries, deserialized notification data can propagate malformed structures, unexpected null values, or unvalidated state transitions into notification dispatch logic, risking silent execution failures or transmitting corrupt payloads to external webhook endpoints.

## Decision

1. MUST: Deserialized notification data MUST be processed through the Zod schema parse method before domain logic accesses any payload attributes.

## Policy Block

- MUST Deserialized notification data MUST be processed through the Zod schema parse method before domain logic accesses any payload attributes.

In scope:
- Incoming serialized notification data received from external or asynchronous triggers.
- Outgoing payload bodies formatted for transmission to external webhook client endpoints.

Out of scope:
- Internal module-to-module function invocations where data has already passed boundary validation.

Exceptions:
- EXC-42-001: An internal high-throughput data stream requires non-blocking processing and boundary validation has already executed upstream.

## Rationale

- Static type definitions do not provide runtime guarantees against malformed JSON data arriving across network or process boundaries.
- Zod provides unified runtime validation and static type inference, eliminating drift between schema constraints and application interfaces.
- Validating outbound payload structures prior to HTTP dispatch prevents transmitting malformed data to third-party consumer endpoints.

## Consequences

Positive:
- Guarantees structural and data type integrity at serialization and external network boundaries.
- Fails fast with structured validation errors before malformed data reaches downstream dispatch logic.
- Maintains alignment between runtime validation and static type definitions through schema inference.

Negative:
- Introduces minor runtime CPU overhead for schema parsing during ingestion and outbound formatting.
- Requires dedicated schema declarations and synchronization whenever payload contracts evolve.

## Alternatives

- Manual conditional assertions and custom type guard functions (rejected)
  Rejected because: Manual type assertions are error-prone, verbose, and difficult to maintain reliably across nested payload structures.
  When valid: Valid only in constrained environments where external dependencies are strictly prohibited.
- Compile-time type assertions without runtime validation (rejected)
  Rejected because: Static type assertions are erased at runtime and provide zero protection against malformed or unexpected serialized data.
  When valid: Valid only across internal module calls within the same process memory space where inputs have already been validated.

## Risks

- Uncaught schema parse exceptions on malformed incoming data could terminate worker execution.
  Mitigation: Enclose schema parse operations in structured exception-handling blocks that record invalid payload diagnostics and prevent process termination.
  Owner: Engineering Team
- Schema drift between event producers and consumers could result in unexpected validation rejections.
  Mitigation: Maintain unified schema definitions across producing and consuming packages within the workspace.
  Owner: Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Export both the Zod schema declarations and their inferred static types from dedicated schema modules to maintain a single source of truth.
- Invoke schema parsing immediately after deserializing external payloads and immediately prior to serializing outbound network request bodies.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and type checking script to verify schema type adherence.
- Discover and run the project automated test suite to confirm schema validation passes on valid payloads and rejects malformed data.

Accept when:
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

## Enforcement

- Verified by: Automated continuous integration pipeline executing static type checks and unit test suites.
- Verified by: Peer code review verifying that external serialization boundaries invoke runtime schema parsing.
- Violation handling: Pull requests that omit runtime schema parsing on external network payloads are rejected until compliant validation and tests are implemented.
- Exception process: Exceptions must be submitted to the architecture review board with documentation of upstream validation guarantees and specific throughput constraints.