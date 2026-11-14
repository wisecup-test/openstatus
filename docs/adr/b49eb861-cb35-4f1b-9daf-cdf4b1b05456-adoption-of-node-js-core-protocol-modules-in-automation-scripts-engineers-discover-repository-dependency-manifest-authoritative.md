---
version: 2
name: b49eb861-cb35-4f1b-9daf-cdf4b1b05456-adoption-of-node-js-core-protocol-modules-in-automation-scripts-engineers-discover-repository-dependency-manifest-authoritative
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: Engineers Discover Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: b49eb861-cb35-4f1b-9daf-cdf4b1b05456
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: Engineers Discover Repository Dependency Manifest Authoritative

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `scripts/**`

## Context

- Workspace packages require utility scripts to automate asset distribution, import transformation, and build coordination.
- Script execution in modern modular environments benefits from explicit module specifiers that distinguish native runtime libraries from external packages.
- Using explicit protocol namespaces for core runtime modules prevents dependency substitution vulnerabilities and ambiguous loader resolution.
- Build-time configuration ingestion relies on direct filesystem access to inspect schemas, compilation settings, and package registries.

## Problem Statement

Build automation and transform scripts across workspace packages require direct access to runtime utilities for filesystem operations and path resolution. Without an explicit import convention, scripts risk ambiguous module resolution, accidental collisions with third-party registry packages, and inconsistent handling across modern modular runtimes.

## Decision

1. MUST: Engineers MUST discover the repository dependency manifest and authoritative lock artifact to verify runtime and engine version constraints before implementing or updating automation scripts.

## Policy Block

- MUST Engineers MUST discover the repository dependency manifest and authoritative lock artifact to verify runtime and engine version constraints before implementing or updating automation scripts.

In scope:
- Build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.
- Automation scripts running directly within the runtime environment.

Out of scope:
- Client-side browser bundle components and UI component definitions.
- Third-party registry packages managed through external dependency declarations.

## Rationale

- Prefixing core module specifiers with the explicit protocol namespace guarantees unambiguous resolution by the runtime loader and eliminates vulnerability to package squatting or shadow dependencies.
- Leveraging standard runtime built-in modules avoids unnecessary third-party utility dependencies for routine filesystem reads and path manipulation tasks.
- Evidence across workspace scripts demonstrates a consistent pattern of synchronous filesystem reading and path transformation to prepare artifacts before downstream consumers execute.

## Consequences

Positive:
- Guarantees deterministic resolution of native runtime modules across workspace automation scripts.
- Reduces external dependency footprint by relying exclusively on standard built-in core capabilities.
- Hardens build security posture against namespace confusion and malicious package shadowing.

Negative:
- Requires developers to maintain awareness of and consistently adhere to the protocol prefix convention across all scripts.
- Precludes script execution on outdated execution runtimes lacking protocol-prefixed specifier resolution.

## Alternatives

- Legacy Bare Core Module Specifiers (rejected)
  Rejected because: Bare specifiers risk collision with third-party packages and do not provide explicit loader-level disambiguation of runtime built-in modules.
  When valid: Valid only when targeting legacy execution environments that predate protocol specifier support.
- External Third-Party Utility Libraries (rejected)
  Rejected because: Introducing external packages for standard filesystem and path operations increases supply chain exposure and maintenance overhead.
  When valid: Valid when complex cross-platform capabilities such as advanced file-watching or custom archival are strictly required.

## Risks

- Inadvertent use of bare specifiers by contributors unfamiliar with protocol namespace conventions.
  Mitigation: Configure repository lint rules and automated verification to reject un-prefixed core module imports.
  Owner: engineering team
- Synchronous file operations causing performance bottlenecks if applied to massive file trees.
  Mitigation: Restrict synchronous operations to initial build configuration loading and bounded script operations.
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
- When authoring workspace automation routines, declare module imports using the explicit protocol specifier prefix to disambiguate built-in runtime modules from external dependencies.
- Ensure build and transformation routines read configuration and schema files synchronously during initial execution and fail early with diagnostic messaging when prerequisites are missing.

## Continuation Context


Verify commands:
- # Discover the workspace validation task from repository configuration and run the script linter
- # Search the script directory to confirm all core module imports use the explicit protocol prefix

Accept when:
- All automation script files importing core runtime capabilities use the explicit protocol namespace without bare specifiers.
- The discovered repository script validation and linting suite completes with zero errors.

## Enforcement

- Verified by: Automated continuous integration checks executing repository script linting rules.
- Verified by: Peer code reviews on all pull requests modifying workspace automation scripts.
- Violation handling: Build pipeline failure on any automation script using bare core module specifiers.
- Violation handling: Mandatory remediation of import syntax prior to merging pull requests.
- Exception process: Formal review and approval by repository maintainers documented in an architectural exception record.