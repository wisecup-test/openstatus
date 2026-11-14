---
version: 2
name: 973388b6-c46c-40e9-b156-5d74dd9bb028-direct-process-environment-variable-access-via-process-module-build-transformation-scripts-access-platform-runtime
description: "Direct Process Environment Variable Access via Process Module: Build Transformation Scripts Access Platform Runtime"
metadata:
  actual:
    adr_id: 973388b6-c46c-40e9-b156-5d74dd9bb028
    status: accepted
    candidate_group_id: 641a54a0-56d3-41a2-a284-dcbd11067dc6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Direct Process Environment Variable Access via Process Module: Build Transformation Scripts Access Platform Runtime

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `scripts/**`

## Context

- Build automation scripts require runtime deployment metadata to coordinate asset transformation.
- The execution environment exposes platform variables directly through runtime process structures.
- Introducing an external configuration parsing library for isolated utility scripts introduces unnecessary dependency weight.

## Problem Statement

Build utility scripts require access to deployment platform context to configure transformation workflows without introducing heavy configuration schema dependencies, creating a tension between lightweight execution and centralized configuration governance.

## Decision

1. MUST: Build and transformation scripts MUST access platform runtime configuration values directly through the global process environment object rather than introducing external configuration management dependencies.

## Policy Block

- MUST Build and transformation scripts MUST access platform runtime configuration values directly through the global process environment object rather than introducing external configuration management dependencies.

In scope:
- Build automation and transformation scripts requiring platform deployment context.
- Local development execution where ambient runtime variables dictate asset resolution.

Out of scope:
- Core runtime application components and business logic modules.
- Components requiring strict schema-validated configuration lifecycles.

Exceptions:
- EXC-37-001: Automated testing environments simulate alternative execution contexts by injecting mock environment properties.

## Rationale

- Direct consumption of process environment properties satisfies transformation needs with zero additional external dependencies.
- Confining direct environment reads to build scripts prevents coupling core application logic to platform-specific environment keys.
- The pattern aligns with lightweight utility requirements where operational context is supplied exclusively by the host execution environment.

## Consequences

Positive:
- Zero external dependency footprint for configuration ingestion in utility workflows.
- Immediate access to ambient platform deployment properties without initialization overhead.

Negative:
- Absence of schema validation can lead to runtime execution failures if expected platform variables are missing.
- Direct references to specific deployment platform variables reduce script portability across heterogeneous execution environments.

## Alternatives

- Adopt a centralized schema validation library across all utility scripts (rejected)
  Rejected because: Introducing schema validation dependencies for simple build utilities adds maintenance overhead disproportionate to isolated single-script scope.
  When valid: When configuration complexity scales across multiple interdependent packages and shared application execution runtimes.
- Pass configuration exclusively via command-line arguments (rejected)
  Rejected because: Deployment environments provide platform metadata automatically through environment variables, requiring redundant wrappers to convert them to command-line parameters.
  When valid: When utilities are invoked interactively by developers rather than automated host pipelines.

## Risks

- Missing platform environment variables can trigger unhandled execution errors during build steps.
  Mitigation: Implement defensive presence checks and explicit error messages when critical variables are undefined.
  Owner: Build and Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that all accessed environment variables are documented in the repository environment documentation.
- Ensure sensitive platform variables are not logged or exposed in transformed output artifacts.

## Continuation Context


Verify commands:
- Discover and run the repository verification scripts to confirm environment configuration parsing complies with project standards.
- Execute repository linting suites to ensure process environment access remains restricted to authorized utility scripts.

Accept when:
- Repository verification scripts pass without reporting unauthorized process environment access.
- Utility scripts successfully parse required platform environment variables and exit cleanly.

## Enforcement

- Verified by: Static analysis checks verifying that direct environment access remains restricted to script scopes.
- Verified by: Peer code review for all modifications to build automation routines.
- Violation handling: Pull requests introducing direct environment reads in production application modules will be blocked during review.
- Exception process: Submit an architectural review request detailing why an application module requires direct process environment access rather than centralized configuration injection.