---
version: 2
name: caf88c90-01dd-458a-877b-7b6b10fdeee3-godotenv-environment-configuration-consumer-inspect-repository-lock-artifact-verify
description: "godotenv Environment Configuration: Consumer Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: caf88c90-01dd-458a-877b-7b6b10fdeee3
    status: accepted
    candidate_group_id: ad5408b7-dbfd-46f7-8b98-2fb9f5252524
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/database/**
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
---

# godotenv Environment Configuration: Consumer Inspect Repository Lock Artifact Verify

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/database/**`
- `internal/server/**`
- `internal/tinybird/**`
- `internal/workflows/**`

## Context

- Runtime configuration sources across the service components directly read ambient environment variables during client instantiation and server initialization.
- Decentralized environment access is distributed across client modules, HTTP server routing, and datastore setup rather than consolidated into a typed configuration structure.
- A third-party environment autoloading dependency is imported at datastore initialization to populate process variables from local configuration files.

## Problem Statement

Decentralized retrieval of configuration variables directly from ambient runtime environments across disparate service components obscures environment requirements, prevents centralized startup validation, and increases risk of runtime failure due to missing configuration values.

## Decision

1. MUST: The consumer MUST inspect the repository lock artifact to verify the exact resolved version of any runtime configuration library before introducing or modifying configuration bindings.

## Policy Block

- MUST The consumer MUST inspect the repository lock artifact to verify the exact resolved version of any runtime configuration library before introducing or modifying configuration bindings.

In scope:
- Internal service components, external API clients, and datastore initializers that consume runtime environment configuration.

Out of scope:
- Static constants and build-time embedded metadata that do not depend on ambient process execution environments.

## Rationale

- Direct environment variable lookups scattered across multiple internal packages create hidden runtime dependencies and make configuration failure troubleshooting difficult.
- Restricting environment file loading to application bootstrapping ensures predictable initialization order across all dependent modules.
- Centralizing configuration parsing allows immediate validation of required environment parameters prior to accepting external traffic.

## Consequences

Positive:
- Clear separation between environment variable loading and component consumption prevents subtle startup race conditions.
- Explicit configuration passing improves unit testability by eliminating global environment state dependencies.
- Missing configuration values are identified early during startup rather than at point-of-use.

Negative:
- Requires boilerplate code to define typed configuration structures and pass them to downstream constructors.
- Developers must coordinate configuration additions through centralized schema definitions.

## Alternatives

- Decentralized on-demand environment variable reading in every component constructor (rejected)
  Rejected because: Scatters environment requirements across the codebase and defers configuration failure discovery to runtime execution.
  When valid: Acceptable only in ephemeral one-off scripts where configuration management overhead outweighs failure isolation.
- Centralized schema-validated configuration management module (deferred)
  Rejected because: Requires establishing a dedicated configuration domain model that spans all repository packages.
  When valid: Recommended when microservice complexity or multi-environment staging requirements grow beyond basic key-value pairs.

## Risks

- Silent failure when optional environment variables are missing during startup
  Mitigation: Enforce explicit default fallback values or fail-fast startup assertions for all required variables
  Owner: engineering team
- Inadvertent exposure of sensitive environment values in structured logging output
  Mitigation: Redact sensitive credential values prior to emitting startup configuration logs
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
- Audit all internal packages for direct ambient environment calls and refactor component constructors to receive explicit parameter objects.
- Ensure environment file initialization occurs in root process entrypoints before invoking any subsystem constructors.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis and linting scripts to identify unmanaged environment lookups.
- Discover and run the project automated test suite to ensure configuration parsing functions conform to expectations.

Accept when:
- Static analysis checks report no unauthorized direct environment access calls in client packages.
- Automated test suites pass with validated configuration inputs.

## Enforcement

- Verified by: Continuous integration static analysis pipelines
- Verified by: Peer review during pull request evaluations
- Violation handling: Pull requests introducing direct environment lookups in business logic components are blocked until refactored.
- Exception process: Documented architectural exemption approved by the engineering team for standalone bootstrap binaries.