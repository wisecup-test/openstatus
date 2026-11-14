---
version: 2
name: 67d83faa-f037-493b-ac0a-97a8c34e2aff-adoption-of-node-fs-module-for-server-side-content-and-asset-ingestion-modules-performing-synchronous-configuration-metadata-loading
description: "Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Modules Performing Synchronous Configuration Metadata Loading"
metadata:
  actual:
    adr_id: 67d83faa-f037-493b-ac0a-97a8c34e2aff
    status: accepted
    candidate_group_id: c198f3a9-7863-4d87-84dc-8c48dbf3824f
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - next.config.ts
      - src/content/**
      - src/lib/**
---

# Adoption of node:fs Module for Server-Side Content and Asset Ingestion: Modules Performing Synchronous Configuration Metadata Loading

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `next.config.ts`
- `src/content/**`
- `src/lib/**`

## Context

- The application requires local reading of documentation files, markdown content, static data tables, and image assets during build and server-rendering cycles.
- Content and configuration data reside within the local project repository alongside application code rather than in a decoupled remote content management service.
- Direct filesystem access enables fast, zero-latency retrieval of static content and asset dimensions without network overhead or third-party service dependencies.

## Problem Statement

The application needs to ingest and validate local content collections, documentation files, build metadata, and static asset dimensions without introducing network latency or external content service dependencies. The architecture must establish a consistent, secure pattern for accessing local disk resources on the server while preventing filesystem module leakage into client-facing bundles.

## Decision

1. MAY: Modules performing synchronous configuration or metadata loading during initialization MAY use readFileSync when asynchronous streaming introduces unnecessary coordination complexity.

## Policy Block

- MAY Modules performing synchronous configuration or metadata loading during initialization MAY use readFileSync when asynchronous streaming introduces unnecessary coordination complexity.

In scope:
- Server-side content ingestion routines, documentation readers, and static asset metadata extractors.
- Build-time configuration scripts and build-phase artifact processors.

Out of scope:
- Client-side rendering components and browser-targeted modules.
- Dynamic database interactions and remote API data fetching operations.

Exceptions:
- EXC-20-001: A module requires dynamic remote asset retrieval where local disk storage is unavailable.

## Rationale

- Importing node:fs directly provides native, dependency-free filesystem access that minimizes external dependencies while operating at local disk speeds.
- Using the explicit node scheme prefix provides unambiguous module resolution, preventing accidental namespace collision with third-party registry packages.
- Centralizing local file ingestion behind server-side utilities ensures that disk operations remain strictly partitioned away from browser execution environments.

## Consequences

Positive:
- Zero network latency and high throughput for content retrieval during build and server-side rendering phases.
- Elimination of external content API operational dependencies and associated credential management for local documentation.
- Simplified local content authoring where documentation and asset changes are version-controlled synchronously with source code.

Negative:
- Disk access is strictly constrained to server and build environments, requiring rigorous module boundary enforcement to prevent bundle compilation failures.
- Synchronous read operations can block the event loop if executed indiscriminately over large files during active server request paths.
- Local file access patterns restrict horizontal scaling across stateless containers if content is not compiled or packaged directly into the deployed artifact.

## Alternatives

- Remote Content Management System API (rejected)
  Rejected because: Introduces external network latency, operational overhead, credential management, and availability dependencies for static content that already resides in the repository.
  When valid: When non-technical content creators require independent publishing workflows decoupled from repository deployments.
- Virtual File System Abstraction Layer (rejected)
  Rejected because: Adds unnecessary architectural complexity and indirection for an application with direct local disk access requirements.
  When valid: When the application must dynamically switch between local disk storage and cloud object storage across disparate deployment targets.

## Risks

- Accidental importation of node:fs into client-side components causing bundle compilation failures.
  Mitigation: Enforce architectural boundary linting and module isolation checks to prevent core runtime imports in client-targeted components.
  Owner: Engineering Team
- Event loop starvation caused by synchronous file read operations on high-frequency request paths.
  Mitigation: Utilize in-memory cache layers for parsed content and restrict synchronous reads to initialization or build-time workflows.
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
- Ensure all imports of the filesystem module employ the node prefix protocol to enforce native runtime resolution.
- Pair filesystem read operations with structured schema validation or safe parsing logic to handle malformed or missing content gracefully.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and linting scripts to verify that node:fs imports are absent from client component directories.
- Discover and run the project's test suite to ensure that all server-side content reading and parsing utilities execute cleanly without filesystem errors.
- Execute the project's build validation routine to confirm that all build-time configuration and asset dimension extractors resolve local files successfully.

Accept when:
- Static analysis confirms that node:fs imports reside strictly within server-side utilities, build configuration, and server rendering components.
- All unit and integration tests for content reading, metadata validation, and asset dimension extraction pass without unhandled filesystem exceptions.

## Enforcement

- Verified by: Automated continuous integration checks verifying module boundary imports and static type safety.
- Verified by: Peer code review for any new module accessing local filesystem resources.
- Violation handling: Pull requests introducing node:fs imports into client-side components or unvalidated read operations will be blocked until remediated.
- Exception process: Submit an architectural exception request to the architecture review board outlining the operational justification and boundary containment plan.