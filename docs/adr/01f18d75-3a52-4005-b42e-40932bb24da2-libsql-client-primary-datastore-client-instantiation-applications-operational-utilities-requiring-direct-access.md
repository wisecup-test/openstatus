---
version: 2
name: 01f18d75-3a52-4005-b42e-40932bb24da2-libsql-client-primary-datastore-client-instantiation-applications-operational-utilities-requiring-direct-access
description: "@libsql/client Primary Datastore Client Instantiation: Applications Operational Utilities Requiring Direct Access"
metadata:
  actual:
    adr_id: 01f18d75-3a52-4005-b42e-40932bb24da2
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Applications Operational Utilities Requiring Direct Access

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `scripts/**`

## Context

- Operational workflows and migration utilities require reliable access to primary relational datastores to inspect and query records.
- Standardizing on `@libsql/client` via `createClient` provides a unified connection interface for relational datastore interactions across operational tasks.
- Decoupling connection configuration into validated runtime variables prevents credential leaks and promotes reproducible connection initialization.

## Problem Statement

Operational utilities and data migration workflows require access to the primary relational datastore to inspect, transform, and verify records. Without a standardized datastore client interface, scripts may adopt fragmented connection mechanisms or insecure credential handling, leading to inconsistent connection lifecycles and unvalidated connection parameters across operational boundaries.

## Decision

1. MUST: Applications and operational utilities requiring direct access to the primary datastore MUST initialize database connections using `@libsql/client` via `createClient`.

## Policy Block

- MUST Applications and operational utilities requiring direct access to the primary datastore MUST initialize database connections using `@libsql/client` via `createClient`.

In scope:
- Standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

Out of scope:
- Client-side browser applications or external analytics endpoints that communicate exclusively through higher-level application programming interfaces.

## Rationale

- Using `@libsql/client` with `createClient` standardizes connection management across primary datastore operations while supporting distributed and embedded deployment topologies.
- Supplying credentials through validated runtime configurations ensures consistent security boundaries across operational workflows.
- A unified client library prevents divergence in query execution patterns across background utilities and production services.

## Consequences

Positive:
- Establishes a standardized mechanism for initializing primary datastore connections across operational tasks.
- Ensures connection strings and authentication tokens undergo runtime validation prior to client creation.
- Provides a consistent query execution interface across datastore interaction workflows.

Negative:
- Couples datastore access routines to the connection semantics and error conventions of `@libsql/client`.
- Requires operational scripts to explicitly manage connection lifecycles and handle transient network latencies during batch execution.

## Alternatives

- Direct socket connections using low-level database network protocols (rejected)
  Rejected because: Bypasses unified client abstractions and requires managing custom socket connections and authentication handshakes.
  When valid: Valid when operating in constrained embedded runtime environments that cannot support standard client library dependencies.
- Ad-hoc HTTP query calls without a dedicated database client library (rejected)
  Rejected because: Lacks structured client-side abstractions, connection pooling, and standardized query preparation interfaces.
  When valid: Valid in isolated lightweight edge environments where dependency footprint must be minimized and single-shot queries suffice.

## Risks

- Unbounded connection creation in batch scripts leading to connection pool exhaustion on the primary datastore.
  Mitigation: Reuse client instances across batch iteration cycles and terminate connections properly upon workflow completion.
  Owner: engineering team
- Mismatched API versions between local client interfaces and remote database engine protocols causing query execution errors.
  Mitigation: Follow version grounding protocols by inspecting resolved dependencies and validating against database protocol documentation.
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
- Initialize `@libsql/client` instances via `createClient` using validated connection URLs and authentication tokens parsed from runtime configurations.
- Implement defensive error handling around datastore queries to manage transient network disconnects during batch data processing.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting verification scripts.
- Discover and run the project type checking verification scripts.
- Discover and run the project test execution suite covering datastore operations.

Accept when:
- Static analysis and type checking verification scripts complete with zero errors.
- Primary datastore client connections instantiate successfully using validated configuration parameters.
- All automated test suites covering datastore operations pass without failures.

## Enforcement

- Verified by: Automated continuous integration checks executing static analysis and dependency validation.
- Verified by: Peer code review verifying that primary datastore connections use the approved client initialization pattern.
- Violation handling: Pull requests introducing unapproved database client drivers or unvalidated connection instantiation patterns will be blocked.
- Violation handling: Flagged code must be refactored to use `@libsql/client` through validated connection parameters.
- Exception process: Submit an architectural exception request outlining why an alternative datastore driver or direct protocol connection is necessary.
- Exception process: Exception requests require review and explicit approval by the data platform and architecture leads prior to implementation.