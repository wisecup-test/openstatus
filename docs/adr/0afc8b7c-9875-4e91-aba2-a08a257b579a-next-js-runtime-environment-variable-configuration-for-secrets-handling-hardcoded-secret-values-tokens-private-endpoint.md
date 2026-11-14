---
version: 2
name: 0afc8b7c-9875-4e91-aba2-a08a257b579a-next-js-runtime-environment-variable-configuration-for-secrets-handling-hardcoded-secret-values-tokens-private-endpoint
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Hardcoded Secret Values Tokens Private Endpoint"
metadata:
  actual:
    adr_id: 0afc8b7c-9875-4e91-aba2-a08a257b579a
    status: accepted
    candidate_group_id: aeae75d1-c923-499e-bbb1-3d56f8c3f818
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - src/**
---

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Hardcoded Secret Values Tokens Private Endpoint

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `src/**`

## Context

- The web application requires integration with external infrastructure services, analytics platforms, monitoring databases, and notification webhooks that demand sensitive credentials and runtime configuration.
- The application framework provides built-in environment variable resolution via process.env across server routes, server components, and client bundles with build-time substitution semantics.
- The codebase exhibits direct access to process.env across application entry points, route handlers, and utility modules without relying on an external secrets management service or dedicated secrets retrieval library.

## Problem Statement

Managing application secrets and runtime credentials across server-side handlers, background tasks, and client interfaces introduces operational complexity and security exposure risks. Without a clear architectural policy governing credentials access, developers may inconsistently manage configuration sources or risk leaking sensitive credentials into client-facing bundles.

## Decision

1. MUST_NOT: Hardcoded secret values, tokens, or private endpoint credentials MUST_NOT be committed into source code.

## Policy Block

- MUST_NOT Hardcoded secret values, tokens, or private endpoint credentials MUST_NOT be committed into source code.

In scope:
- Server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

Out of scope:
- Static assets and client-side styling definitions that do not interact with runtime configuration or external service credentials.

## Rationale

- Direct reliance on the framework's native process.env interface avoids introducing additional runtime dependencies or external secrets manager SDK overhead into the application.
- Standardizing on process.env aligns with platform hosting conventions where deployment environments inject configuration variables directly into the process runtime.
- Maintaining direct access keeps configuration lightweight across route handlers, though it defers schema validation and secret rotation capabilities to external platform controls.

## Consequences

Positive:
- Eliminates third-party client dependency overhead for secrets fetching, reducing cold-start latency and operational footprint.
- Aligns cleanly with platform hosting environments that inject configuration directly into process execution contexts.
- Provides a uniform, universally recognized mechanism for reading runtime environment flags and credentials across server modules.

Negative:
- Lacks centralized compile-time or startup schema validation, allowing missing environment variables to fail at the point of execution rather than during startup.
- Increases the risk of accidental exposure of server-only secrets if variable naming conventions for client-exposed values are improperly applied.
- Couples secret rotation to process redeployment or restart cycles because values are evaluated directly from the execution environment.

## Alternatives

- Adopt an external centralized secrets manager SDK to fetch credentials dynamically at runtime (rejected)
  Rejected because: Introducing runtime network calls to external secrets management services adds latency, introduces network failure modes during request processing, and increases infrastructure complexity.
  When valid: When dynamic secret rotation without service redeployment or fine-grained audit logging per secret access is an explicit compliance mandate.
- Implement an isolated schema-validated configuration module encapsulating all process.env accesses (rejected)
  Rejected because: The current architectural baseline directly reads process.env at call sites across routes and utilities, and adopting an encapsulated schema module would require refactoring existing variable consumption patterns.
  When valid: When strict startup validation is required to fail fast if any required environment variable is missing or malformed.

## Risks

- Accidental client-side exposure of sensitive server credentials through improper variable naming or shared module imports.
  Mitigation: Enforce strict separation between client and server modules and validate that client bundles never import server-exclusive configuration variables.
  Owner: engineering team
- Runtime failure when an optional or required environment variable is undefined in a newly deployed environment.
  Mitigation: Incorporate presence checks or fallback validation logic in critical execution paths handling external service authentication.
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
- Developers must consult the official framework documentation corresponding to the exact locked framework version to verify environment variable inlining behavior across server and client boundaries.
- All secrets required for outbound external authentication should be retrieved via process.env immediately prior to request construction, ensuring sensitive values are not retained in global mutable state.

## Continuation Context


Verify commands:
- Discover and execute the project code analysis scripts to detect unauthorized hardcoded credentials or invalid environment variable references.
- Discover and execute the project automated test suites to confirm route handlers correctly handle configured and unconfigured external service endpoints.

Accept when:
- Static code analysis confirms no hardcoded secret strings or private credential tokens are committed in source files.
- Automated test suites pass without referencing undefined critical environment variables.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline scanning for hardcoded secrets.
- Verified by: Peer code review verifying that sensitive variables on process.env are never referenced in client-rendered components.
- Violation handling: Pull requests containing hardcoded credentials or leaking server environment variables to client contexts are blocked from merging.
- Violation handling: Immediate credential revocation and rotation if a secret token is detected in committed code.
- Exception process: Submit a formal security architecture exception request detailing the necessity of third-party secrets SDK integration, subject to review and sign-off by the lead security architect.