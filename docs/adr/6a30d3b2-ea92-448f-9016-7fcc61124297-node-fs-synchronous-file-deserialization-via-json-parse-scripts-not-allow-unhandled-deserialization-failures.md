---
version: 2
name: 6a30d3b2-ea92-448f-9016-7fcc61124297-node-fs-synchronous-file-deserialization-via-json-parse-scripts-not-allow-unhandled-deserialization-failures
description: "node:fs Synchronous File Deserialization via JSON.parse: Scripts Not Allow Unhandled Deserialization Failures"
metadata:
  actual:
    adr_id: 6a30d3b2-ea92-448f-9016-7fcc61124297
    status: accepted
    candidate_group_id: 896a6963-08d0-4e2c-8aae-65bf89cf6ad6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# node:fs Synchronous File Deserialization via JSON.parse: Scripts Not Allow Unhandled Deserialization Failures

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `scripts/**`

## Context

- Build-time transformation scripts require configuration metadata to resolve module paths and component registries.
- File access and parsing are implemented using built-in runtime modules and direct JSON deserialization routines without an intermediate schema validation layer.
- Configuration ingestion is isolated to local transformation workflows during script execution.

## Problem Statement

Direct deserialization of configuration files using standard runtime parsing primitives lacks schema validation, exposing build routines to uncaught syntax and structure exceptions when configuration inputs deviate from expected shapes.

## Decision

1. MUST_NOT: Scripts MUST_NOT allow unhandled deserialization failures from JSON.parse to terminate execution without emitting diagnostic location details for the invalid configuration artifact.

## Policy Block

- MUST_NOT Scripts MUST_NOT allow unhandled deserialization failures from JSON.parse to terminate execution without emitting diagnostic location details for the invalid configuration artifact.

In scope:
- Internal build and transformation scripts performing configuration file ingestion via runtime file system APIs.

Out of scope:
- Client-facing application bundles and runtime browser execution contexts.

## Rationale

- Direct usage of node:fs and JSON.parse provides lightweight, zero-dependency ingestion of configuration data for localized build transformations.
- Establishing defensive parsing constraints prevents silent script aborts and opaque runtime errors during automated asset transformations.
- Retaining minimal runtime built-in boundaries avoids adding unnecessary external dependencies to the workspace when simple validation suffices.

## Consequences

Positive:
- Eliminates third-party dependency overhead for internal script execution.
- Ensures predictable failure modes when configuration inputs contain malformed syntax.
- Preserves fast execution characteristics for build-time asset processing.

Negative:
- Requires explicit defensive checks in the absence of a shared runtime validation framework.
- Leaves deep object structure validation unenforced unless explicitly scripted.

## Alternatives

- Adoption of an external schema validation framework across all build scripts (rejected)
  Rejected because: Introduces external dependencies and build overhead for isolated transformation scripts where basic error handling provides sufficient stability.
  When valid: When configuration schemas cross network boundaries or accept unvetted external input.
- Unchecked direct property access on raw parsed JSON structures (rejected)
  Rejected because: Leaves scripts vulnerable to unhandled syntax errors and unexpected property reference failures when configuration files are misconfigured.
  When valid: Never in automated production build pipelines.

## Risks

- Missing or malformed properties in configuration files causing unexpected transformation failures without explicit schema enforcement.
  Mitigation: Implement explicit defensive property checks or structural assertions on parsed configuration objects.
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
- Ensure file read operations using node:fs specify explicit text encoding prior to passing content to deserialization routines.
- Enclose JSON.parse calls in structured error-handling blocks to capture syntax exceptions and emit diagnostic context.

## Continuation Context


Verify commands:
- # Discover repository validation scripts and execute the configured test suite
- # Inspect project configuration to locate and run static analysis checks across build scripts

Accept when:
- All build scripts ingest configuration files without throwing unhandled deserialization exceptions.
- The repository verification suite executes without syntax or property access errors across transformed assets.

## Enforcement

- Verified by: Automated continuous integration build checks
- Verified by: Peer code review for changes affecting build and transformation scripts
- Violation handling: Pull requests introducing unhandled deserialization routines in build scripts are blocked until defensive error handling is added.
- Exception process: Exceptions require documented justification and sign-off from the technical lead when scripts operate exclusively on static local fixtures.