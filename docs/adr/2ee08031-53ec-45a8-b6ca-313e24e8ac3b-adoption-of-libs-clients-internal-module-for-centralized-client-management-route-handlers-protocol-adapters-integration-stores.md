---
version: 2
name: 2ee08031-53ec-45a8-b6ca-313e24e8ac3b-adoption-of-libs-clients-internal-module-for-centralized-client-management-route-handlers-protocol-adapters-integration-stores
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Route Handlers Protocol Adapters Integration Stores"
metadata:
  actual:
    adr_id: 2ee08031-53ec-45a8-b6ca-313e24e8ac3b
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Route Handlers Protocol Adapters Integration Stores

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Server routing endpoints across REST, MCP, RPC, and Slack messaging integrations require access to shared backing services and datastores.
- Direct instantiation of connection clients within individual endpoint files causes configuration divergence, unmanaged connection proliferation, and tight coupling between transport handlers and infrastructure drivers.
- Static analysis identifies a uniform import pattern where route handlers, protocol adapters, and agent stores delegate client access to the centralized internal module @/libs/clients.

## Problem Statement

Decentralized instantiation of backing service and infrastructure clients within individual route handlers and adapters leads to duplicated client configurations, connection pooling inefficiencies, and inconsistent error handling across server transport layers.

## Decision

1. MUST: Route handlers, protocol adapters, integration stores, and agent runners MUST import backing infrastructure and service client instances exclusively from @/libs/clients.

## Policy Block

- MUST Route handlers, protocol adapters, integration stores, and agent runners MUST import backing infrastructure and service client instances exclusively from @/libs/clients.

In scope:
- All server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

Out of scope:
- Isolated utility modules, static pure functions, and client-side web application codebases that do not interact with server backing services.

## Rationale

- Centralizing client instantiation in @/libs/clients provides a single control point for connection pooling, environment configuration, and credentials management across diverse transport boundaries.
- Evidence across six distinct server routes and adapters demonstrates that shared client access prevents boilerplate duplication and ensures uniform telemetry and error behaviors.
- Decoupling route handlers from client driver construction simplifies handler unit testing and enables centralized client reconfiguration without modifying routing files.

## Consequences

Positive:
- Eliminates duplicate client connection initialization across disparate route, adapter, and integration modules.
- Centralizes connection lifecycle, configuration parsing, and pooling strategies within a single internal module.
- Standardizes client access patterns across heterogeneous protocols including REST, RPC, MCP, and Slack messaging.

Negative:
- Creates an architectural coupling where all route modules depend directly on the @/libs/clients module interface contract.
- Requires updates to the central module whenever a route or adapter requires specialized, non-standard client configuration options.

## Alternatives

- Direct in-file client instantiation within route handlers (rejected)
  Rejected because: Produces fragmented connection pools, scattered credential configuration, and inconsistent client initialization across endpoints.
  When valid: Valid only in standalone single-file prototype scripts with no shared infrastructure requirements.
- Context-injected client instances through dynamic dependency injection containers (deferred)
  Rejected because: Introduces unnecessary container overhead and indirection beyond the operational needs of current route handlers.
  When valid: Valid when dynamic per-request tenant client isolation or multi-container lifecycle swapping is required.

## Risks

- A failure or improper initialization inside the central client module can propagate across all route handlers.
  Mitigation: Enforce rigorous health checks and defensive initialization within the client module alongside comprehensive test validation.
  Owner: Backend Engineering Team
- Over-centralization could encourage leaking driver-specific internals into routes that do not need them.
  Mitigation: Export well-defined interfaces and keep underlying driver configuration private within the client module.
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
- Expose pre-configured singleton client instances from the central module to ensure connection reuse across request lifecycles.
- Ensure that client configurations reference validated runtime environment schemas rather than reading raw process environment variables within route handlers.

## Continuation Context


Verify commands:
- Discover and run the repository architectural lint scripts to confirm all client imports originate from the designated module.
- Run the project test suite to verify route and adapter integration with client instances.

Accept when:
- All imports of backing infrastructure clients across server route files and adapters resolve to @/libs/clients.
- No route, adapter, or store file instantiates external connection drivers or client libraries directly.
- The repository test suite and module verification checks pass without architectural import errors.

## Enforcement

- Verified by: Automated continuous integration checks inspecting import paths and module dependencies.
- Verified by: Peer code reviews validating compliance with client consumption rules during pull request evaluation.
- Violation handling: Pull requests containing direct client driver instantiations outside the designated module will be blocked during automated verification.
- Violation handling: Violations identified in active development must be refactored to import from the centralized client module before merge approval.
- Exception process: Submit an architectural review request detailing why the centralized client module cannot accommodate the specific connection configuration or lifecycle requirement.
- Exception process: Secure approval from the backend engineering team before introducing alternative client initialization paths.