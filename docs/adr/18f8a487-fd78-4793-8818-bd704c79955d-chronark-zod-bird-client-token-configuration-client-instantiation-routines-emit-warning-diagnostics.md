---
version: 2
name: 18f8a487-fd78-4793-8818-bd704c79955d-chronark-zod-bird-client-token-configuration-client-instantiation-routines-emit-warning-diagnostics
description: "@chronark/zod-bird Client Token Configuration: Client Instantiation Routines Emit Warning Diagnostics"
metadata:
  actual:
    adr_id: 18f8a487-fd78-4793-8818-bd704c79955d
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Client Instantiation Routines Emit Warning Diagnostics

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `src/**`

## Context

- Analytics client integrations require authentication tokens to query remote data pipelines.
- Hardcoding credentials in client instantiation logic poses significant security and compliance risks.
- Applications must support development and testing workflows where live analytics credentials may not be configured.

## Problem Statement

Analytics client integrations require secure secret token propagation from runtime environments into client instances without hardcoding credentials or breaking development workflows when tokens are unconfigured.

## Decision

1. SHOULD: Client instantiation routines SHOULD emit warning diagnostics and return empty data structures when secret tokens are omitted in non-production environments.

## Policy Block

- SHOULD Client instantiation routines SHOULD emit warning diagnostics and return empty data structures when secret tokens are omitted in non-production environments.

In scope:
- Client initialization and credential provisioning for analytics pipes.
- Modules managing external API secrets and authentication tokens.

Out of scope:
- Public or unauthenticated client endpoints that require no secret credentials.
- Static public assets and client-side code where secrets must not be exposed.

## Rationale

- Adopting explicit token injection into @chronark/zod-bird Client instances keeps secrets decoupled from application code and compliant with security policies.
- Emitting diagnostic warnings upon missing tokens enables non-blocking local development while clearly flagging unconfigured pipes.

## Consequences

Positive:
- API tokens and secrets remain isolated from version-controlled source files.
- Client initialization supports environment-specific credential injection and endpoint re-routing.
- Missing credentials trigger informative diagnostic warnings without terminating development workflows.

Negative:
- Applications must handle unauthenticated states gracefully to avoid silent query failures where empty data is returned.
- External runtime environment configuration must be explicitly maintained across all deployment environments.

## Alternatives

- Static Client-Level Secret Embedding (rejected)
  Rejected because: Embedding API keys directly in client source files creates credential leakage vulnerabilities and prevents environment-specific authorization rotation.
  When valid: Never valid in version-controlled applications.
- Mandatory Process Halt on Missing Credentials (rejected)
  Rejected because: Halting execution immediately upon missing analytics credentials blocks auxiliary services and non-analytics testing workflows in development environments.
  When valid: Valid only when analytics querying is a mandatory blocking prerequisite for application initialization.

## Risks

- Silent query degradation when missing credentials cause empty response datasets without fatal exceptions.
  Mitigation: Emit diagnostic warnings when credentials are absent and validate token presence during deployment verification.
  Owner: Engineering Team
- Inadvertent secret exposure through broad environment logging or error propagation.
  Mitigation: Scope credential extraction to specific environment keys without serializing full environment dictionaries into logs.
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
- Inject client tokens through application configuration factories or environment readers rather than referencing raw global state directly within business logic.
- Ensure warning handlers or fallback pipelines alert operators when token configuration is absent so degraded analytics responses can be differentiated from empty query results.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and secret detection checks to ensure no hardcoded API keys exist in client integration code.
- Discover and execute the test suite for client initialization to verify that missing credentials trigger appropriate diagnostic warnings and fallback behavior.

Accept when:
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

## Enforcement

- Verified by: Continuous integration static secret scanning.
- Verified by: Peer code review on client and analytics configuration modules.
- Verified by: Automated unit and integration test execution.
- Violation handling: Pull requests containing hardcoded credentials or unvalidated token access are blocked.
- Violation handling: Detected secrets are immediately revoked and rotated.
- Exception process: Exceptions must be submitted via formal architecture review and require sign-off from security leads.