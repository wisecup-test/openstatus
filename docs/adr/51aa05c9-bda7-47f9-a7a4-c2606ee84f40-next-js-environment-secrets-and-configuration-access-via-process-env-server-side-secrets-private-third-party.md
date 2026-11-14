---
version: 2
name: 51aa05c9-bda7-47f9-a7a4-c2606ee84f40-next-js-environment-secrets-and-configuration-access-via-process-env-server-side-secrets-private-third-party
description: "Next.js Environment Secrets and Configuration Access via process.env: Server Side Secrets Private Third Party"
metadata:
  actual:
    adr_id: 51aa05c9-bda7-47f9-a7a4-c2606ee84f40
    status: accepted
    candidate_group_id: af28e293-b25b-4e4f-9a84-6964ae34513c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Next.js Environment Secrets and Configuration Access via process.env: Server Side Secrets Private Third Party

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/**`

## Context

- Distributed runtime components, server route handlers, and client interfaces require configuration values and third-party credentials to authenticate external services and toggle environment behavior.
- Direct invocation of runtime environment variables across disparate modules introduces exposure risks if client and server execution boundaries are conflated.
- Framework conventions isolate client-exposed variables from server-only secrets through explicit naming prefixes during compilation and asset bundling.

## Problem Statement

Applications requiring third-party credentials and environment flags across both client components and server execution boundaries risk accidental secret leakage and runtime misconfiguration when configuration access is decentralized without clear boundary segregation.

## Decision

1. MUST: Server-side secrets and private third-party integration keys MUST be accessed exclusively within server runtime boundaries through process.env without exposing values to client bundles.

## Policy Block

- MUST Server-side secrets and private third-party integration keys MUST be accessed exclusively within server runtime boundaries through process.env without exposing values to client bundles.

In scope:
- All application source code accessing environment configuration, third-party authentication tokens, API secrets, and client-exposed identifiers.
- Server API route handlers, proxy boundaries, authentication provider configurations, and client components consuming environment toggles.

Out of scope:
- Static assets and styling definitions that do not interact with runtime environment configurations.
- Third-party dependency internals that manage their own isolated configuration mechanisms.

## Rationale

- Direct referencing of process.env aligns with standard twelve-factor application architecture by segregating dynamic environment configuration from immutable source code.
- Enforcing strict separation between private environment variables and public-prefixed variables prevents accidental bundling of private authentication keys and webhook secrets into client distributions.
- Decentralized runtime reads across twenty-six files demonstrate reliance on execution-environment injection for deployment flexibility across development, preview, and production stages.

## Consequences

Positive:
- Eliminates hardcoded credentials from version control while supporting environment-specific runtime configurations.
- Maintains explicit boundary separation between public client-side telemetry identifiers and private backend authentication tokens.
- Enables standard containerized and serverless deployments to inject configuration dynamically without recompilation.

Negative:
- Decentralized property lookups can lead to runtime execution failures if mandatory environment variables are omitted during deployment.
- Lacks centralized static type definitions for configuration keys without supplemental schema validation wrappers.

## Alternatives

- Centralized configuration module wrapping process.env with runtime schema validation (deferred)
  Rejected because: While providing compile-time type safety and startup validation, it requires refactoring all direct variable access points across the codebase.
  When valid: Valid when application complexity requires strict startup validation fail-fast guarantees.
- Hardcoded configuration constants with environment branch switching (rejected)
  Rejected because: Commits sensitive secrets and deployment-specific identifiers into version control, violating fundamental security compliance requirements.
  When valid: Never valid for secrets or sensitive runtime credentials.

## Risks

- Accidental prefixing of private secrets with the public client prefix causing credential leakage in browser assets
  Mitigation: Automate build-time asset inspection and static analysis checks to reject public prefixes on sensitive token definitions
  Owner: Security & Compliance Team
- Missing environment variables causing unexpected runtime null pointer exceptions in production execution paths
  Mitigation: Implement validation schemas at application initialization boundaries to verify required environment keys prior to handling traffic
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
- Verify that all private authentication tokens and webhook verification secrets reside solely in server execution contexts such as route handlers and server utilities.
- Ensure that variables required in browser environments adhere strictly to the framework public variable naming convention.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and security scanning tasks to detect exposed secrets or invalid environment variable references.
- Discover and execute the project build and compilation routines to ensure environment variable bundling rules are respected without leakage.

Accept when:
- Static security scanning completes with zero findings of unencrypted secrets or misplaced credentials.
- Client-side compilation artifacts contain zero references to private server secrets and environment tokens.

## Enforcement

- Verified by: Automated continuous integration pipeline executing security scanning and build verification checks.
- Verified by: Mandatory peer code review for any pull request modifying environment variable access or configuration files.
- Violation handling: Pull requests introducing direct leakage of server secrets or unverified environment reads are blocked from merging.
- Violation handling: Build pipeline immediately terminates if sensitive credentials are detected in client bundles.
- Exception process: Exceptions for non-standard configuration loading must receive written sign-off from the security lead and be documented in the repository issue tracker.