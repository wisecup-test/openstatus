---
version: 2
name: 79707ff4-b0ba-447f-96c1-35c908438382-adoption-of-embed-package-for-in-memory-asset-integration-consumers-inspect-dependency-manifest-lock-artifact
description: "Adoption of embed Package for In-Memory Asset Integration: Consumers Inspect Dependency Manifest Lock Artifact"
metadata:
  actual:
    adr_id: 79707ff4-b0ba-447f-96c1-35c908438382
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Consumers Inspect Dependency Manifest Lock Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/ssh-server`

Applies to:
- `**` (whole root)

## Context

- Service entrypoints require bundled configuration and template assets alongside standard payload serialization and stream handling.
- The application incorporates the core embed package to bind internal static data into the compiled executable unit.
- Associated standard utilities including encoding/json and io handle structured transformations without introducing third-party runtime dependencies.

## Problem Statement

Standalone service executables require dependable access to bundled assets and structured serialization without introducing external file path dependencies or third-party package footprints.

## Decision

1. MUST: Consumers MUST inspect the dependency manifest and lock artifact in the repository to confirm toolchain support for the embed package before implementation.

## Policy Block

- MUST Consumers MUST inspect the dependency manifest and lock artifact in the repository to confirm toolchain support for the embed package before implementation.

In scope:
- Service entrypoints and compiled executables requiring bundled static assets and standard serialization handling.

Out of scope:
- Dynamic external resources and distributed asset stores that require modification independently of executable releases.

## Rationale

- Adopting the standard embed package eliminates external filesystem path coupling and guarantees that compiled binaries remain atomic and self-contained.
- Relying on standard core packages for serialization and input/output stream management minimizes supply-chain surface area and dependency maintenance overhead.
- Static analysis detects localized usage within an entrypoint component, indicating targeted utility for executable packaging.

## Consequences

Positive:
- Eliminates runtime missing-asset failures by encapsulating necessary resources directly within the binary.
- Maintains a minimal dependency footprint by relying entirely on core library modules.
- Simplifies service deployment by producing self-contained single-artifact deliverables.

Negative:
- Increases compiled executable binary size proportionally to the volume of embedded content.
- Requires complete rebuild and redeployment cycles to update any packaged asset content.

## Alternatives

- Filesystem Path Resolution at Runtime (rejected)
  Rejected because: Introduces runtime working-directory sensitivity, potential missing-file errors, and synchronization overhead during deployments.
  When valid: When assets must be modified or replaced dynamically without recompiling the executable artifact.
- External Asset Packaging Libraries (rejected)
  Rejected because: Introduces third-party dependency management overhead and external build toolchains when standard core library primitives satisfy the requirement.
  When valid: When virtual filesystem features such as runtime write capability or proprietary compression algorithms are strictly required.

## Risks

- Excessive binary artifact bloat caused by inlining large files into compilation units.
  Mitigation: Establish asset size limits and restrict in-binary embedding strictly to lightweight schemas, metadata, and core templates.
  Owner: Engineering Team
- Deployment of stale assets resulting from unmonitored file modifications bypassing the build pipeline.
  Mitigation: Configure repository build definitions to monitor asset paths and trigger recompilation upon content changes.
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
- Place embed directives directly adjacent to package-level variable declarations and expose content via standard read-only interfaces.
- Couple asset decoding routines with package initialization checks to validate data schema integrity early in the process lifecycle.

## Continuation Context


Verify commands:
- Discover the project test runner from repository manifests and execute the test suite to verify embedded asset loading.
- Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors.

Accept when:
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

## Enforcement

- Verified by: Automated continuous integration pipelines executing repository test and build suites.
- Verified by: Peer code review verifying that asset encapsulation relies on embed declarations rather than runtime relative paths.
- Violation handling: Code changes that introduce runtime filesystem dependencies for static service assets must be rejected during review and refactored.
- Exception process: Submit an architectural review request documenting asset volume constraints or runtime mutability requirements that necessitate external storage solutions.