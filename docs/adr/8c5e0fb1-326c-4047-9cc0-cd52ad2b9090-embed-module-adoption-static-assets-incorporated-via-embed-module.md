---
version: 2
name: 8c5e0fb1-326c-4047-9cc0-cd52ad2b9090-embed-module-adoption-static-assets-incorporated-via-embed-module
description: "embed Module Adoption: Static Assets Incorporated Via Embed Module"
metadata:
  actual:
    adr_id: 8c5e0fb1-326c-4047-9cc0-cd52ad2b9090
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Static Assets Incorporated Via Embed Module

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/ssh-server`

Applies to:
- `**` (whole root)

## Context

- The server application includes compile-time asset embedding capabilities alongside standard serialization, formatted output, and logging utilities.
- Static assets required for server initialization must be accessible in distributed runtime environments without relying on external filesystem paths.
- The observed pattern is localized to a single service entrypoint, demonstrating isolated usage of core embedding and utility modules rather than an organization-wide standard.

## Problem Statement

Services distributing binary artifacts often face runtime failures when external configuration or static asset files are decoupled from executable binaries, creating dependencies on local filesystem paths that may vary across deployment environments.

## Decision

1. MUST: Static assets incorporated via the embed module MUST be accessed through standard in-memory file system interfaces rather than external relative file paths.

## Policy Block

- MUST Static assets incorporated via the embed module MUST be accessed through standard in-memory file system interfaces rather than external relative file paths.

In scope:
- Standalone services that compile static assets directly into binaries.
- Components requiring immutable embedded file systems.

Out of scope:
- Components managing dynamic or frequently rotated external resources.
- Modules performing streaming or external network file transfers.

## Rationale

- Embedding assets into compiled binaries ensures self-contained deployments and eliminates runtime filesystem missing-file errors.
- The embed package provides a standard in-memory filesystem interface that integrates directly with standard input/output abstractions without external library dependencies.
- The pattern is currently evidenced in a single service entrypoint, representing a local implementation choice rather than an established multi-service cross-cutting convention.

## Consequences

Positive:
- Executable binaries are self-contained with all necessary assets bundled at compile time.
- Eliminates runtime errors caused by missing asset files or relative filesystem path differences.
- Avoids introducing third-party dependencies for static asset management.

Negative:
- Binary size increases proportionally with the size of embedded static assets.
- Asset updates necessitate a complete service rebuild and redeployment rather than in-place asset updates.

## Alternatives

- Filesystem-relative asset loading at runtime (rejected)
  Rejected because: Relies on ambient filesystem structures and relative paths that frequently fail in containerized or varying deployment environments.
  When valid: Valid when assets are massive or updated independently of service binary deployment cycles.
- External configuration service or remote storage fetching (rejected)
  Rejected because: Introduces external network dependencies and latency during service startup for static invariable files.
  When valid: Valid for dynamic runtime configurations or shared assets updated across multiple services.

## Risks

- Unchecked inclusion of large static assets can bloat binary distribution sizes.
  Mitigation: Enforce asset size limits during code review and review binary size metrics in the automated build pipeline.
  Owner: engineering team
- Accidental embedding of sensitive credentials or secrets into distributed binaries.
  Mitigation: Conduct automated static secret scanning and strict peer review on embedded directories.
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
- Discover the repository layout to identify asset directories designated for compile-time embedding before applying directives.
- Ensure assets to be embedded are restricted to immutable static resources required strictly for service startup and core operation.

## Continuation Context


Verify commands:
- Discover and run the project's standard build and test suite scripts from the repository configuration to confirm asset compilation.
- Execute repository static analysis and security scanning checks discovered from the workspace configuration.

Accept when:
- The service compiles successfully with all embedded assets verified by automated tests.
- No relative filesystem path dependencies remain for bundled static assets.

## Enforcement

- Verified by: Automated build pipeline checks validating binary compilation and test execution.
- Verified by: Peer code review verifying appropriate usage of embed directives and exclusion of sensitive files.
- Violation handling: Pull requests attempting to embed dynamic secrets or excessively large binaries will be blocked during review.
- Violation handling: Build failures triggered by missing embedded assets must be resolved before merging.
- Exception process: Exceptions for services requiring dynamic asset resolution must be documented in a pull request approved by the lead architect.