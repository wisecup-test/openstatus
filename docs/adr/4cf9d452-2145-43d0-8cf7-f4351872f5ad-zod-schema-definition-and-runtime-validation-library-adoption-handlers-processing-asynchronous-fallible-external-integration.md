---
version: 2
name: 4cf9d452-2145-43d0-8cf7-f4351872f5ad-zod-schema-definition-and-runtime-validation-library-adoption-handlers-processing-asynchronous-fallible-external-integration
description: "Zod Schema Definition and Runtime Validation Library Adoption: Handlers Processing Asynchronous Fallible External Integration"
metadata:
  actual:
    adr_id: 4cf9d452-2145-43d0-8cf7-f4351872f5ad
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Handlers Processing Asynchronous Fallible External Integration

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- The application exposes multiple entry points including HTTP routes, RPC adapters, external integration webhooks, and tool registries.
- Untrusted external input received across diverse protocols requires rigorous runtime verification before invocation of internal service methods.
- Static TypeScript types alone cannot validate untrusted runtime payloads arriving at system boundaries.
- A unified, declarative approach to schema definition and validation was required to prevent inconsistent manual validation logic.

## Problem Statement

Boundary entry points receiving untrusted external payloads across HTTP, RPC, webhooks, and tool invocation registries require deterministic runtime validation; relying on unchecked static type casts leads to runtime exceptions and security vulnerabilities when external inputs diverge from expected structures.

## Decision

1. SHOULD: Handlers processing asynchronous or fallible external integration payloads SHOULD utilize safeParse to manage validation errors without throwing unhandled runtime exceptions.

## Policy Block

- SHOULD Handlers processing asynchronous or fallible external integration payloads SHOULD utilize safeParse to manage validation errors without throwing unhandled runtime exceptions.

In scope:
- All external HTTP route handlers and middleware.
- All RPC service adapters and protocol converters.
- All external platform integration command and webhook receivers.
- All dynamic tool registry execution adapters.
- All application environment configuration loaders.

Out of scope:
- Internal private functions operating strictly downstream of validated boundary data.
- Performance-isolated computational routines operating on pre-allocated primitive buffers.

Exceptions:
- EXC-20-001: Internal pure algorithmic utilities operating exclusively on previously parsed and typed domain entities.

## Rationale

- Evidence across twelve files demonstrates pervasive adoption of Zod across route handlers, RPC adapters, Slack commands, tool registries, and environment parsing.
- Zod allows bi-directional safety by generating compile-time TypeScript types from runtime schema definitions, eliminating type divergence.
- Employing declarative parse and safeParse methods ensures consistent error structures and centralized constraint validation at ingress boundaries.

## Consequences

Positive:
- Guarantees runtime type safety and strict schema conformance at application and protocol boundaries.
- Eliminates duplication by deriving static TypeScript types directly from runtime schema definitions.
- Standardizes error reporting and validation semantics across diverse integration channels.
- Prevents malformed or malicious payloads from penetrating downstream internal domain services.

Negative:
- Introduces minor runtime CPU parsing overhead on high-throughput ingress endpoints.
- Increases bundle size by including runtime validation logic alongside type declarations.
- Requires schema maintenance alongside evolving protocol specifications.

## Alternatives

- Manual TypeScript type assertion guards and procedural validation functions (rejected)
  Rejected because: Manual validation guards lack automated runtime type inference, duplicate type declarations, and frequently lead to human oversight in boundary checking.
  When valid: Valid only in isolated performance-critical loops with microsecond latency requirements where object allocation is prohibited.
- Separate JSON schema generation with external validation engines (rejected)
  Rejected because: Decoupled JSON schemas create synchronization overhead between static TypeScript interfaces and dynamic validation definitions.
  When valid: Valid when schema contracts must be shared with foreign language platforms lacking TypeScript interoperability.

## Risks

- Computational overhead during parsing of large or deeply nested payloads.
  Mitigation: Validate payloads exclusively at ingress boundaries rather than across internal module transitions.
  Owner: Engineering Team
- Schema definitions diverging from upstream external API contracts.
  Mitigation: Implement comprehensive integration tests that validate schema definitions against external protocol payloads.
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
- Construct composable schema building blocks for shared domain attributes to promote reuse across distinct route definitions and protocol adapters.
- Derive internal data contracts directly from schema definitions using type inference rather than maintaining standalone interface declarations.
- Handle safeParse outcomes at network boundary controllers to transform schema mismatch issues into standardized client error responses.

## Continuation Context


Verify commands:
- Discover and execute the project test runner script defined in the repository dependency manifest to verify schema validation test suites pass.
- Discover and execute the static type verification script declared in the repository workspace configuration to ensure all schema-inferred types compile without errors.
- Discover and execute the project linter and format validation scripts to verify adherence to validation standards across all route and adapter files.

Accept when:
- All schema validation test suites pass successfully across boundary adapters and route handlers.
- Static type analysis passes with zero diagnostic errors across all modules defining or consuming schema-inferred types.
- Runtime input parsing reliably rejects malformed boundary payloads and permits valid payloads across all integration entry points.

## Enforcement

- Verified by: Automated continuous integration test and static type verification suites.
- Verified by: Peer code reviews verifying schema coverage on all new boundary entry points and integration handlers.
- Violation handling: Continuous integration build failures on unvalidated boundary parameters or type mismatches.
- Violation handling: Rejection of pull requests attempting to parse untrusted payloads without standardized schema validation.
- Exception process: Submit an architectural review request detailing performance requirements or external serialization constraints.
- Exception process: Obtain formal sign-off from the technical lead prior to introducing alternative boundary validation mechanisms.
- Exception process: Document the approved deviation and boundary containment strategy in the module architecture notes.