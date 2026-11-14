---
version: 2
name: 04698e16-73a7-41a6-925c-1487b4dfafeb-zod-schema-definition-for-runtime-boundary-validation-and-data-contract-enforcement-runtime-boundaries-that-ingest-untrusted-external
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Runtime Boundaries That Ingest Untrusted External"
metadata:
  actual:
    adr_id: 04698e16-73a7-41a6-925c-1487b4dfafeb
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Runtime Boundaries That Ingest Untrusted External

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/**`

## Context

- External input data enters the application through multiple ingress surfaces including HTTP route parameters, request payloads, remote API integration responses, and environment variables.
- Compile-time static type systems verify internal data structures during development but provide zero runtime validation against malformed, omitted, or malicious payloads delivered across system boundaries.
- Unchecked incoming payloads and unvalidated remote responses introduce vulnerabilities, data corruption risks, and unpredictable failure cascades across asynchronous execution flows.
- The application codebase requires a declarative, composable runtime validation solution that binds runtime boundary verification directly to static type definitions without manual type duplication.

## Problem Statement

The application consumes dynamic, untrusted payloads across server endpoint handlers, third-party remote API invocations, environment configuration loaders, and content utilities. Relying solely on compile-time static type definitions leaves system boundaries unprotected against unvalidated or mutated structures at runtime, leading to silent propagation of invalid data, unhandled runtime exceptions, and degraded fault isolation.

## Decision

1. MUST: All runtime boundaries that ingest untrusted or external data MUST declare authoritative data schemas using Zod validation primitives.

## Policy Block

- MUST All runtime boundaries that ingest untrusted or external data MUST declare authoritative data schemas using Zod validation primitives.

In scope:
- Incoming HTTP server route handlers, query parameter parsers, and request payload consumers
- External client responses received from remote service integrations and third-party APIs
- Application environment variable definitions and configuration loaders
- Content parsing, search indexing, and domain-level structured schemas

Out of scope:
- Internal private functions operating strictly within verified domain boundaries where inputs have already been validated
- Static assets and constant data structures compiled directly into the codebase without dynamic external mutation

## Rationale

- Analysis across application entry points, remote service clients, and configuration modules demonstrates pervasive usage of Zod schemas to guarantee input safety and fail-closed data validation.
- Coupling runtime schema validation with static type inference eliminates drift between runtime boundary reality and compile-time contract assumptions, preventing invalid inputs from polluting internal domain logic.
- Standardizing boundary verification around safeParse mechanisms allows routes to return structured validation diagnostics without propagating unhandled exceptions or exposing internal system internals.

## Consequences

Positive:
- Deterministic runtime validation ensures malformed or unexpected data structures fail at the system perimeter before entering business logic.
- Automatic static type inference from runtime schemas reduces boilerplate and prevents desynchronization between types and validation rules.
- Standardized validation behavior provides consistent error reporting across all application endpoints and integrations.

Negative:
- Runtime validation introduces computational CPU overhead on high-throughput data paths parsing complex nested payloads.
- Schema maintenance requires developers to define explicit boundary schemas for all incoming data models, increasing initial authoring effort.
- Changes to external provider response contracts necessitate synchronized updates to validation schemas to avoid rejecting valid responses.

## Alternatives

- Manual Type Guard Functions and Ad-Hoc Validation Logic (rejected)
  Rejected because: Writing custom type predicate functions and manual conditional checks is error-prone, duplicates type definitions, and lacks standardized error reporting.
  When valid: Valid only in constrained environments where zero external dependencies are permitted or for trivial primitive assertions.
- Compile-Time-Only Type Assertions (rejected)
  Rejected because: Type assertions erase completely at runtime, providing zero protection against malformed external inputs or unexpected schema drift from remote services.
  When valid: Valid only within internal closed modules where data provenance has already been proven by prior boundary validation.

## Risks

- Parsing overhead on extremely large or nested payloads may degrade endpoint latency under heavy concurrent request volume.
  Mitigation: Design schemas to validate only necessary boundary fields, avoiding deep recursive parsing on throughput-sensitive routes.
  Owner: Backend engineering team
- External service upstream contract drift may cause validation to reject unexpected schema variations, causing integration disruptions.
  Mitigation: Implement defensive schema definitions with nullable and optional field tolerances for third-party responses, backed by automated integration tests.
  Owner: Integration engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Place shared data validation schemas within dedicated schema modules accessible by both server routes and client services to maximize schema reuse.
- Prefer safeParse over parse in request handling paths to generate structured diagnostic responses rather than throwing caught exceptions.
- Use schema inference utilities provided by the library to export static types alongside schema objects to maintain a single source of truth.

## Continuation Context


Verify commands:
- Execute the repository type-checking pipeline discovered from workspace configuration to ensure all schema-inferred types conform to internal interfaces.
- Execute the test runner discovered from project automation scripts to verify that invalid inputs correctly trigger validation failures across all defined schemas.
- Execute the linting and static analysis suite discovered from repository configuration to detect any unchecked type assertions across boundary files.

Accept when:
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

## Enforcement

- Verified by: Automated continuous integration pipelines executing type-checking and test verification suites on all pull requests.
- Verified by: Peer code review verifying that new entry points, remote clients, and environment variables include schema validation.
- Violation handling: Pull requests containing unvalidated boundary inputs or manual type casts at external boundaries are blocked from merging.
- Violation handling: Detected contract violations or missing schemas require author remediation before deployment.
- Exception process: Exceptions for performance-critical high-frequency internal paths require documented benchmark rationale and approval by an architectural lead.