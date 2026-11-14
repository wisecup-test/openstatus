---
version: 2
name: af49d291-d12c-4fda-8f5f-6edd9e7ca35a-adoption-of-openstatus-error-for-server-error-standardization-error-schemas-serialization-logic-incorporate-code
description: "Adoption of @openstatus/error for Server Error Standardization: Error Schemas Serialization Logic Incorporate Code"
metadata:
  actual:
    adr_id: af49d291-d12c-4fda-8f5f-6edd9e7ca35a
    status: accepted
    candidate_group_id: f4bfe830-6739-435c-8463-26208c562aeb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/errors/**
      - src/routes/rpc/interceptors/**
---

# Adoption of @openstatus/error for Server Error Standardization: Error Schemas Serialization Logic Incorporate Code

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/libs/errors/**`
- `src/routes/rpc/interceptors/**`

## Context

- Server services expose capabilities through both HTTP endpoints and RPC procedures requiring predictable error reporting across disparate protocols.
- Client consumers require standardized error responses containing actionable error codes, documentation references, and correlation identifiers for troubleshooting.
- Inconsistent error structures across transport boundaries increase integration friction and impede centralized observability and debugging.

## Problem Statement

Disparate error representations between HTTP and RPC interfaces create inconsistent client contracts, hinder automated documentation generation, and complicate cross-service tracing when unhandled exceptions or domain errors occur.

## Decision

1. MUST: Error schemas and error serialization logic MUST incorporate error code enumerations from @openstatus/error and associate each error with a correlation identifier extracted from the request context.

## Policy Block

- MUST Error schemas and error serialization logic MUST incorporate error code enumerations from @openstatus/error and associate each error with a correlation identifier extracted from the request context.

In scope:
- All server route handlers, RPC procedure implementations, and transport interceptors emitting error responses
- Error utility modules defining API error schemas and response structures

Out of scope:
- Internal utility functions that do not cross service or transport boundaries
- Client-side applications consuming API responses

## Rationale

- Adopting @openstatus/error establishes a single source of truth for error codes and structural models across all server transport mechanisms.
- Centralizing error definitions ensures error responses automatically provide consistent documentation links and correlation identifiers.
- Shared error schemas prevent divergence between HTTP OpenAPI specifications and RPC interceptor behavior.

## Consequences

Positive:
- Establishes a uniform error taxonomy across all communication protocols including HTTP and RPC
- Enforces consistent inclusion of correlation identifiers and documentation references for API consumers
- Centralizes error schema generation for documentation and client specification generation

Negative:
- Requires all consuming modules to declare dependency on the shared error package and conform to its type contracts
- Adding or refining error codes requires updating the central library rather than defining local constants

## Alternatives

- Ad-hoc error throwing using native Error instances within individual route handlers (rejected)
  Rejected because: Leads to inconsistent error response payloads, missing correlation identifiers, and leaking internal runtime details across transport boundaries
  When valid: Small standalone utility scripts with no remote clients or external consumers
- Protocol-specific error models maintained independently for HTTP and RPC transports (rejected)
  Rejected because: Duplicates error taxonomy definitions and creates divergent error code semantics between different transport interfaces
  When valid: Services implementing isolated protocols without shared clients or overlapping operational domains

## Risks

- Centralized error package modifications could trigger widespread downstream rebuilds across consuming packages.
  Mitigation: Design error code additions to be backward compatible and maintain stable interface contracts within the shared error library.
  Owner: Platform Engineering
- Developers might map unexpected errors to generic codes instead of defining precise domain error representations.
  Mitigation: Establish linting rules and code review checkpoints to ensure domain-specific error mapping.
  Owner: API Development Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define new error classifications directly within @openstatus/error to maintain centralized cataloging before exposing them across service endpoints.
- Ensure RPC interceptors and HTTP error middleware consistently attach request correlation identifiers to both structured log records and outgoing error payloads.

## Continuation Context


Verify commands:
- Discover and run the repository type-checking script across server packages to verify strict type conformity for all @openstatus/error exports and usages.
- Discover and execute the repository unit and integration test suite to validate error formatting and RPC interceptor behavior.
- Execute the repository linting and static analysis suite to ensure error interceptors adhere to structured logging and error serialization rules.

Accept when:
- Type checking passes without errors, confirming all referenced error codes and schema objects conform to @openstatus/error definitions.
- Unit and integration tests pass, confirming HTTP and RPC error responses emit standardized error codes and correlation identifiers.
- Static analysis validates that unexpected errors are intercepted and logged with correlated request identifiers.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository test and type-checking suites on pull requests
- Verified by: Architectural and peer code reviews evaluating route handlers and transport interceptor changes
- Violation handling: Pull requests introducing raw, unclassified error responses or bypassing @openstatus/error schemas are blocked from merging.
- Violation handling: Code review comments require developers to map custom error conditions to standard @openstatus/error codes.
- Exception process: Exceptions for non-standard transport protocols require review and approval by the platform engineering lead with documented tracking.