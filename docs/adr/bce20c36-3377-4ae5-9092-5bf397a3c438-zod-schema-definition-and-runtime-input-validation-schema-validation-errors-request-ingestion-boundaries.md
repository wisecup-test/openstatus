---
version: 2
name: bce20c36-3377-4ae5-9092-5bf397a3c438-zod-schema-definition-and-runtime-input-validation-schema-validation-errors-request-ingestion-boundaries
description: "Zod Schema Definition and Runtime Input Validation: Schema Validation Errors Request Ingestion Boundaries"
metadata:
  actual:
    adr_id: bce20c36-3377-4ae5-9092-5bf397a3c438
    status: accepted
    candidate_group_id: 74d602d4-ffd9-4e37-b923-9b6d92d150fb
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - env.mjs
      - script/**
      - src/schema/**
---

# Zod Schema Definition and Runtime Input Validation: Schema Validation Errors Request Ingestion Boundaries

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `env.mjs`
- `script/**`
- `src/schema/**`

## Context

- External payloads, workspace limit overrides, relational migration scripts, and runtime environment values enter the persistence tier from untrusted or dynamically typed boundaries.
- Unvalidated data entering storage operations risks data corruption, constraint violations, and runtime exceptions within downstream service logic.
- Static type checking does not guarantee runtime payload conformity when deserializing dynamic data structures such as serialized payloads or environment parameters.
- The codebase requires a standardized, composable schema validation mechanism to guarantee strict type inference and runtime validation across all data access boundaries.

## Problem Statement

Data access layers and persistence schemas frequently ingest dynamic, untrusted payloads, including environment configurations, workspace limit definitions, audit records, and migration inputs. Relying solely on compile-time types fails to protect runtime boundaries from malformed, missing, or out-of-spec values, which can lead to silent database corruption, runtime type errors, or security vulnerabilities.

## Decision

1. SHOULD: Schema validation errors at request or ingestion boundaries SHOULD be handled using safe parsing routines that capture structured error details for logging and error reporting without unhandled exceptions.

## Policy Block

- SHOULD Schema validation errors at request or ingestion boundaries SHOULD be handled using safe parsing routines that capture structured error details for logging and error reporting without unhandled exceptions.

In scope:
- Domain entity schema declarations and relational model validation definitions.
- Workspace limit definitions, audit log payloads, and session message validation.
- Environment variable ingestion and database migration script input parameters.

Out of scope:
- Pure internal functions operating entirely on previously validated internal types.
- Transient in-memory structures generated and consumed within a single validated boundary.

Exceptions:
- EXC-42-001: A performance-critical batch processing pipeline has already validated inputs at an upstream boundary and must avoid redundant parsing operations.

## Rationale

- Adopting Zod standardizes runtime input validation across database schema modules, ensuring that runtime data strictly matches expected structural specifications.
- Zod provides bidirectional synergy between runtime validation schemas and compile-time type inference, eliminating drift between type definitions and validation logic.
- Centralizing validation logic in dedicated schema modules adjacent to entity definitions prevents fragmented, ad-hoc validation logic across disparate database access queries.

## Consequences

Positive:
- Enforces strict runtime type boundaries, preventing corrupted or malformed data from persisting to storage.
- Derives static types directly from validation schemas, ensuring type safety and reducing manual synchronization overhead.
- Provides structured validation errors that facilitate precise diagnostic reporting and logging.

Negative:
- Adds runtime parsing CPU overhead on high-throughput ingestion and data transformation paths.
- Requires maintaining schema definitions alongside relational table mappings, introducing architectural surface area.

## Alternatives

- Manual imperative validation functions for domain models (rejected)
  Rejected because: Imperative validation checks are prone to omission, lack uniform error structures, and require manual maintenance of separate static type interfaces.
  When valid: Valid only in lightweight standalone scripts with minimal dependencies and no requirement for static type inference.
- Compile-time only type assertions (rejected)
  Rejected because: Compile-time type assertions are erased at runtime and provide zero protection against malformed external data or corrupted serialized values.
  When valid: Valid only for internal code paths where data origins are guaranteed to be fully validated.

## Risks

- Validation schema divergence from underlying database storage constraints leading to unexpected parsing failures.
  Mitigation: Derive validation schemas directly from table definitions using schema integration helpers and verify alignment through schema integration tests.
  Owner: Engineering Team
- Performance degradation when repeatedly parsing deeply nested data structures during high-volume reads.
  Mitigation: Apply schema parsing primarily at untrusted mutation boundaries rather than repeatedly validating immutable internal records.
  Owner: Data Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the schema definitions directory in the data layer package and ensure all entity validation schemas export inferred static types alongside schema objects.
- When validating untrusted input that may fail under normal user flows, prefer safe parsing methods to handle failures gracefully without throwing uncaught exceptions.

## Continuation Context


Verify commands:
- Discover and execute the repository type-check script to validate schema definitions and type inference across all database schema modules.
- Discover and execute the project test runner against the schema validation test suite to confirm all validation rules and parsing constraints pass.
- Discover and execute the static analysis linter to verify that no untrusted input bypasses schema validation.

Accept when:
- All schema validation test suites execute and pass with zero assertion failures across all schema modules.
- Static type checking completes with zero type errors across all derived schema types and parsing invocations.
- Boundary ingestion points reject malformed payloads with structured validation errors while accepting conformant data.

## Enforcement

- Verified by: Continuous integration pipelines enforcing automated test execution and static type checks across all modules.
- Verified by: Peer code reviews verifying that new data persistence endpoints and environment parameters define and apply validation schemas.
- Violation handling: Pull requests introducing unvalidated ingestion boundaries or type assertions without schema parsing will be blocked.
- Violation handling: Detected schema bypasses must be refactored to introduce explicit schema validation before merging.
- Exception process: Exceptions require formal submission of a justification outlining performance or structural rationale.
- Exception process: Approval must be granted by the lead architect or security officer and documented in the architecture exception register.