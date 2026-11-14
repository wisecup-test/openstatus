---
version: 2
name: 95d44c7a-3ebf-4e92-8001-7e1137239dcd-process-env-environment-variable-access-when-introducing-third-party-validation-configuration
description: "process.env Environment Variable Access: When Introducing Third Party Validation Configuration"
metadata:
  actual:
    adr_id: 95d44c7a-3ebf-4e92-8001-7e1137239dcd
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: When Introducing Third Party Validation Configuration

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `scripts/**`

## Context

- Build-time scripts require access to deployment environment variables to resolve environment state and service endpoints.
- Direct invocation of process.env is observed within utility scripts handling artifact transformation.
- Static analysis detects unvalidated property access on the global process runtime object rather than consumption through an abstracted configuration schema.

## Problem Statement

Direct property access on the runtime environment object introduces unvalidated dependencies on deployment environment state during build and transform steps, creating configuration drift risk and lacking contract verification.

## Decision

1. MUST: When introducing third-party validation or configuration management libraries, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version prior to use.

## Policy Block

- MUST When introducing third-party validation or configuration management libraries, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version prior to use.

In scope:
- Build and transformation scripts executing in the workspace that access runtime environment variables.

Out of scope:
- Client-side bundle code and component definitions that run in external user agents.

## Rationale

- Direct property reads from process.env provide zero-dependency access to environment variables in build automation.
- The pattern is localized to execution scripts rather than production application bundles, limiting runtime credential exposure.
- The single-file occurrence represents an isolated script utility rather than an established cross-cutting architectural standard for secrets management.

## Consequences

Positive:
- Avoids external dependency overhead for lightweight build transformation scripts.
- Provides direct access to deployment environment metadata supplied by the hosting platform.

Negative:
- Lacks type-safe validation and centralized schema definitions for environment variables.
- Increases the likelihood of silent failures when expected environment variables are absent.

## Alternatives

- Centralized Schema-Validated Configuration Module (rejected)
  Rejected because: Introducing a dedicated schema parsing layer adds unnecessary complexity and dependencies for isolated build scripts.
  When valid: When environment variables are shared across multiple application services requiring strict runtime guarantees.
- Hardcoded Build Configuration Constants (rejected)
  Rejected because: Hardcoding environment metadata prevents dynamic resolution across different deployment stages.
  When valid: When values are invariant across all deployment targets and testing tiers.

## Risks

- Missing or malformed environment variables cause silent misconfigurations or script aborts during builds.
  Mitigation: Implement defensive assertion checks that verify critical environment keys before script processing begins.
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
- Verify that all required environment variables are defined in the deployment environment configuration before running transformation pipelines.
- Ensure scripts handling sensitive configuration do not print raw environment variable values to standard output logs.

## Continuation Context


Verify commands:
- Discover the workspace script verification target in the package manifest and execute the designated test or lint suite.
- Inspect build script sources to verify that all process.env accesses include existence checks or fallback handling.

Accept when:
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and lint workflows.
- Verified by: Peer code review for changes affecting build and transformation scripts.
- Violation handling: Pull requests introducing unhandled process.env dereferencing are blocked until fallback or validation logic is added.
- Exception process: Exceptions must be documented in the pull request description and approved by the engineering lead.