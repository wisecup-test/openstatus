---
version: 2
name: 62271283-4026-40b7-9276-aefdc1a0bf3d-tsup-library-adoption-for-component-package-bundling-consumers-inspect-repository-lock-artifact-determine
description: "Tsup Library Adoption for Component Package Bundling: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 62271283-4026-40b7-9276-aefdc1a0bf3d
    status: accepted
    candidate_group_id: 297ea402-2b1a-4e79-b4c6-b361293ac201
    evidence:
      source: deep
  scope:
    root: packages/react
---

# Tsup Library Adoption for Component Package Bundling: Consumers Inspect Repository Lock Artifact Determine

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/react`

Applies to:
- `**` (whole root)

## Context

- Workspace library packages require compilation and bundling to produce consumable distribution artifacts.
- Publishable component packages require distinct module output formats and type declarations for diverse consumer environments.
- Adopting a lightweight, dedicated module bundler simplifies package build definitions and prevents build configuration drift.

## Problem Statement

Workspace component packages need a standardized, low-overhead mechanism to compile TypeScript source code into distributed module formats without duplicating complex build configuration across packages.

## Decision

1. MUST: Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the packaging library before building or packaging artifacts.

## Policy Block

- MUST Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the packaging library before building or packaging artifacts.

In scope:
- Workspace library packages that require compiled distribution artifacts for consumers.

Out of scope:
- Full-stack application entrypoints and standalone services governed by application-level bundlers.

## Rationale

- Adopting tsup provides zero-configuration defaults for library builds, generating compatible module formats and declaration files efficiently.
- Isolating packaging configuration within the package boundary decouples individual package artifact creation from repository-wide build systems.

## Consequences

Positive:
- Standardizes module artifact generation across library packages with minimal configuration overhead.
- Enables consistent dual-format distribution and declaration file generation for downstream package consumers.

Negative:
- Introduces an additional packaging dependency and configuration surface for workspace packages.
- Build output differences between module formats require explicit verification across target runtimes.

## Alternatives

- Raw TypeScript compilation without bundling (rejected)
  Rejected because: Fails to produce bundled distribution formats and requires consumers to handle module resolution and asset bundling directly.
  When valid: Valid for purely internal workspaces where consuming applications compile all source files directly from source.
- General-purpose application bundlers (rejected)
  Rejected because: Introduces significant configuration overhead and complex asset pipelines unnecessary for discrete component libraries.
  When valid: Valid for full-stack applications or single-page apps requiring complex HTML and static asset bundling pipelines.

## Risks

- Discrepancies between bundler output formats and package export maps may lead to runtime resolution failures in consuming applications.
  Mitigation: Verify that module export maps match the formats emitted by the bundling configuration through automated verification tests.
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
- Discover build entry points and target output formats from the package dependency manifest and workspace configuration.
- Ensure that generated declaration artifacts and module bundles align with package manifest export declarations.

## Continuation Context


Verify commands:
- # Discover and execute the package build script defined in the repository configuration
REPO_BUILD_SCRIPT=$(grep -l '"build"' package.json */package.json 2>/dev/null | head -n 1)
test -n "$REPO_BUILD_SCRIPT" && echo "Package build entrypoint discovered"
- # Verify that the package distribution output directory is produced upon build execution
test -d dist || echo "Verify distribution output directory creation post-build"

Accept when:
- The package build task compiles and outputs module bundles according to the package bundling configuration.
- The package build process executes without module resolution or compilation errors.

## Enforcement

- Verified by: Automated continuous integration build checks validating package compilation.
- Verified by: Peer code review of package configuration and manifest definitions.
- Violation handling: Build pipeline failure when package build configurations deviate from approved packaging definitions.
- Violation handling: Rejection of pull requests introducing unapproved packaging tools into workspace packages.
- Exception process: Submit an architectural exception request detailing why the standard packaging library cannot meet package artifact distribution requirements.
- Exception process: Obtain approval from the architecture review group prior to introducing alternative packaging pipelines.