---
version: 2
name: e57220c9-074b-47b6-aff7-1734aa7451b1-adopt-zod-for-runtime-assertion-schema-validation-and-parsing-assertion-parsing-logic-not-bypass-runtime
description: "Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Parsing Logic Not Bypass Runtime"
metadata:
  actual:
    adr_id: e57220c9-074b-47b6-aff7-1734aa7451b1
    status: accepted
    candidate_group_id: 2433e36a-68e4-4bbc-93f7-a891b38dda85
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# Adopt Zod for Runtime Assertion Schema Validation and Parsing: Assertion Parsing Logic Not Bypass Runtime

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/assertions`

Applies to:
- `src/**`

## Context

- The assertions subsystem processes heterogeneous assertion data structures that undergo serialization and deserialization across module boundaries.
- Unchecked deserialization of assertion payloads risks introducing silent type corruption and unhandled runtime exceptions.
- The codebase establishes runtime validation schemas across assertion definition, serialization, and type representation modules.

## Problem Statement

Heterogeneous assertion payloads transmitted or stored as serialized data require guaranteed structural conformance at ingestion boundaries. Relying solely on static type assertions risks runtime corruption from malformed payloads, while fragmented manual validation introduces maintenance overhead and schema drift across serialization boundaries.

## Decision

1. MUST_NOT: Assertion parsing logic MUST_NOT bypass runtime schema validation by casting raw deserialized objects directly to domain interfaces.

## Policy Block

- MUST_NOT Assertion parsing logic MUST_NOT bypass runtime schema validation by casting raw deserialized objects directly to domain interfaces.

In scope:
- Assertion payload definitions and structural type declarations.
- Deserialization and ingestion boundaries handling serialized assertion data.

Out of scope:
- Internal domain operations executing on already-parsed and validated assertion instances.
- Non-assertion modules that do not handle serialized assertion contracts.

## Rationale

- Centralizing runtime schema definitions using zod provides authoritative validation and structured parsing for discriminated assertion types.
- Enforcing schema parsing at deserialization boundaries guarantees that invalid payloads are caught immediately before entering domain logic.
- Inferring static types directly from runtime schemas eliminates synchronization discrepancies between static interface declarations and runtime data contracts.

## Consequences

Positive:
- Guarantees runtime structural conformance for all serialized and deserialized assertion payloads at system boundaries.
- Maintains automated alignment between static types and runtime validation contracts through schema inference.
- Provides deterministic parsing and failure reporting when malformed assertion payloads are ingested.

Negative:
- Incurs runtime computational overhead during schema parsing of large payload collections.
- Introduces direct architectural coupling to the zod runtime validation library across assertion modules.

## Alternatives

- Manual type guards and custom validation functions (rejected)
  Rejected because: Manual type guards require duplicate maintenance alongside static type declarations and lack composable declarative parsing capabilities.
  When valid: In execution environments where zero third-party runtime dependencies are strictly mandated.
- External JSON Schema definitions with code generation (rejected)
  Rejected because: JSON Schema requires external schema definitions and an auxiliary generation step to derive static types, increasing build complexity.
  When valid: When schemas must be shared across heterogeneous programming language runtimes.

## Risks

- Uncaught parsing exceptions may halt execution if malformed payloads are ingested without boundary guards.
  Mitigation: Implement structured error handling around boundary parsing invocations to handle malformed payloads without unhandled runtime terminations.
  Owner: engineering team
- Synchronous schema parsing on high-volume assertion collections could introduce latency bottlenecks.
  Mitigation: Perform validation strictly at system entry boundaries and pass validated domain instances internally to prevent redundant validation cycles.
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
- Assemble composite assertion schemas from atomic field validators using discriminated object schemas keyed on an assertion type attribute.
- Execute schema parsing immediately upon deserializing serialized string data to catch structural violations before domain processing occurs.

## Continuation Context


Verify commands:
- Discover the test runner from the project manifest and execute the test suite covering assertion schema validation and serialization.
- Discover the static analysis tool from the project configuration and verify type compatibility between inferred schema types and assertion handlers.

Accept when:
- All assertion payload parsing tests pass without schema validation regressions.
- Static type checking completes with zero type errors across assertion schema definitions.

## Enforcement

- Verified by: Automated test suites executing schema parsing validation across serialized assertion payloads.
- Verified by: Code review verification that all newly introduced assertion contracts export and consume zod schemas.
- Violation handling: Pull requests that ingest unvalidated assertion structures or use manual type casting without schema parsing will be rejected.
- Exception process: Exceptions require formal architectural review and documented justification when non-standard validation mechanisms are necessary.