---
version: 2
name: ffb064ca-557e-4bc9-ac06-659a4233300d-zod-schema-definition-and-runtime-validation-data-transformation-validation-routines-execute-parsing
description: "Zod Schema Definition and Runtime Validation: Data Transformation Validation Routines Execute Parsing"
metadata:
  actual:
    adr_id: ffb064ca-557e-4bc9-ac06-659a4233300d
    status: accepted
    candidate_group_id: b8eea867-4633-4dd5-b6aa-39b93daf18b4
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation: Data Transformation Validation Routines Execute Parsing

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/telegram`

Applies to:
- `src/**`

## Context

- External integration boundaries receive untyped or dynamic payloads requiring runtime verification before downstream execution.
- The codebase defines configuration and communication schemas utilizing Zod declarative schema builders.
- Static type annotations alone do not provide runtime boundary enforcement against malformed inputs.

## Problem Statement

External communication adapters and integration channels process payloads originating from outside the application boundary. Without automated runtime schema validation, malformed or unexpected data shapes can bypass compile-time guarantees, leading to runtime exceptions and unpredictable failure modes.

## Decision

1. SHOULD: Data transformation and validation routines SHOULD execute parsing at boundary ingress before passing validated structures to domain handlers.

## Policy Block

- SHOULD Data transformation and validation routines SHOULD execute parsing at boundary ingress before passing validated structures to domain handlers.

In scope:
- Channel configuration schemas and external payload parsing boundaries within integration packages.

Out of scope:
- Internal domain entity state transitions that do not interface with untrusted or external data boundaries.

## Rationale

- Static analysis identifies Zod object and primitive declarations defining channel contracts at boundary ingress points.
- Declarative runtime schema definition unifies type inference with runtime validation, eliminating divergence between runtime checks and compile-time types.
- Centralized schema declarations in dedicated schema modules provide single-source-of-truth contracts for integration configurations.

## Consequences

Positive:
- Guarantees runtime structural integrity for incoming channel configurations and payloads.
- Generates accurate static type definitions directly from validation schemas without duplicate interface declarations.
- Fails fast at integration boundaries with structured validation errors when schemas are violated.

Negative:
- Introduces runtime execution overhead during schema parsing for incoming payloads.
- Couples integration modules to an external validation library dependency across boundary contracts.

## Alternatives

- Manual conditional type-guard checks (rejected)
  Rejected because: Manual type guards are error-prone, verbose, and difficult to maintain as schema complexity increases.
  When valid: Lightweight utility functions with trivial scalar inputs where external dependencies are strictly prohibited.
- Compile-time only type definitions without runtime validation (rejected)
  Rejected because: Compile-time type annotations are erased at runtime and offer zero protection against malformed external payloads.
  When valid: Purely internal cross-module calls within trust boundaries where data is already validated.

## Risks

- Performance degradation if complex schemas are repeatedly instantiated or evaluated in hot execution loops.
  Mitigation: Instantiate schemas statically as reusable module-level singletons and parse only at boundary entry points.
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
- Define reusable schemas as module-level constants to prevent redundant allocation during payload processing.
- Infer TypeScript types directly from schemas rather than manually writing independent interface definitions.

## Continuation Context


Verify commands:
- Discover the project test runner from the root manifest and run unit tests covering schema validation.
- Discover the repository type check script and verify that schema inference types align with expected contract interfaces.

Accept when:
- All unit tests exercising schema parsing and boundary rejection pass successfully.
- Static type analysis confirms zero type mismatch errors between inferred schema types and boundary consumer signatures.

## Enforcement

- Verified by: Automated test suite execution in continuous integration.
- Verified by: Peer code review verifying runtime validation schemas at new boundary ingress points.
- Violation handling: Pull requests lacking runtime schema validation for external payload boundaries will be blocked.
- Violation handling: Failing schema validation tests will halt pipeline progression.
- Exception process: Exceptions require documented approval from the architecture team explaining alternative validation mechanisms.