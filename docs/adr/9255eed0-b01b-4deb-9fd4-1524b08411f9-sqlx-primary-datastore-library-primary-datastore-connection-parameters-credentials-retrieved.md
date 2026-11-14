---
version: 2
name: 9255eed0-b01b-4deb-9fd4-1524b08411f9-sqlx-primary-datastore-library-primary-datastore-connection-parameters-credentials-retrieved
description: "sqlx Primary Datastore Library: Primary Datastore Connection Parameters Credentials Retrieved"
metadata:
  actual:
    adr_id: 9255eed0-b01b-4deb-9fd4-1524b08411f9
    status: accepted
    candidate_group_id: 3af012f4-f5a3-46a8-bb9b-577ab0ca762b
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/database/**
      - internal/tinybird/**
---

# sqlx Primary Datastore Library: Primary Datastore Connection Parameters Credentials Retrieved

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/database/**`
- `internal/tinybird/**`

## Context

- The backend service requires structured access to a primary relational datastore with connection lifecycle management.
- Static analysis detected sqlx datastore connection initialization alongside a separate HTTP-based telemetry client within internal service packages.

## Problem Statement

Internal services requiring primary datastore connectivity need a consistent mechanism for initializing connection pools, managing credentials securely, and mapping relational queries without introducing fragmented datastore abstractions.

## Decision

1. MUST: Primary datastore connection parameters and credentials MUST be retrieved from runtime environment variables rather than embedded in source definitions.

## Policy Block

- MUST Primary datastore connection parameters and credentials MUST be retrieved from runtime environment variables rather than embedded in source definitions.

In scope:
- Relational primary datastore connection initialization and query lifecycle management within internal backend services.

Out of scope:
- External HTTP analytics endpoints, third-party REST API integrations, and ephemeral client telemetry streams.

## Rationale

- Adopting sqlx provides lightweight extensions over standard database interfaces without enforcing heavy object-relational mapping conventions.
- Centralizing connection construction and closure through standardized lifecycle methods ensures uniform resource allocation and cleanup.

## Consequences

Positive:
- Standardizes relational datastore access patterns across internal service packages.
- Preserves compatibility with standard database connection interfaces while adding structured row scanning.

Negative:
- Restricts backend services from utilizing alternative object-relational mapping frameworks without explicit architecture exceptions.
- Requires manual connection pooling and transaction lifecycle orchestration.

## Alternatives

- Adopt a full-featured Object-Relational Mapping (ORM) framework (rejected)
  Rejected because: Introduces significant abstraction overhead, hidden query generation, and complex runtime metadata tracking unnecessary for internal services.
  When valid: Valid when application domains require complex entity relationship graphs and multi-dialect schema migration tooling.
- Standard database library without extension utilities (rejected)
  Rejected because: Lacks convenient struct-scanning helpers and named query parameter binding, resulting in repetitive boilerplate.
  When valid: Valid when zero external dependencies is a strict operational mandate.

## Risks

- Inadvertent connection pool leakage if consumer components fail to invoke termination routines on application shutdown.
  Mitigation: Enforce connection lifecycle boundaries through standardized constructor and shutdown hooks verified during integration testing.
  Owner: Backend Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project configuration mechanism to verify environment variable names for primary datastore connection strings and authentication tokens before instantiating clients.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting verification suite to confirm datastore connections utilize approved constructor interfaces.
- Discover and execute the project unit and integration test suite to validate datastore connection initialization and cleanup routines.

Accept when:
- Static verification confirms all relational datastore access points use sqlx connection abstractions without unapproved database drivers.
- Integration tests confirm datastore initialization and teardown routines execute cleanly without resource leaks.

## Enforcement

- Verified by: Automated continuous integration checks inspecting datastore package dependencies.
- Verified by: Peer code review for any new datastore client implementations or configuration changes.
- Violation handling: Pull requests introducing unapproved datastore drivers or direct unencapsulated connections are blocked until remediated.
- Exception process: Architecture review board submission detailing performance or operational constraints justifying an alternative datastore client.