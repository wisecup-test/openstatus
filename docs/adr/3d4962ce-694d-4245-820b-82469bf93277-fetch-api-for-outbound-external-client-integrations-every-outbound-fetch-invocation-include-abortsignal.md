---
version: 2
name: 3d4962ce-694d-4245-820b-82469bf93277-fetch-api-for-outbound-external-client-integrations-every-outbound-fetch-invocation-include-abortsignal
description: "Fetch API for Outbound External Client Integrations: Every Outbound Fetch Invocation Include Abortsignal"
metadata:
  actual:
    adr_id: 3d4962ce-694d-4245-820b-82469bf93277
    status: accepted
    candidate_group_id: a984bfff-2085-4149-9bfa-0170b11bd480
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/lib/**
      - src/router/**
---

# Fetch API for Outbound External Client Integrations: Every Outbound Fetch Invocation Include Abortsignal

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/lib/**`
- `src/router/**`

## Context

- The backend API layer coordinates with diverse external targets including worker nodes, notification endpoints, and third-party management services.
- Service boundaries require authenticated HTTP communication with structured payload serialization and deterministic timeout controls.
- The codebase consistently utilizes the native fetch API across routing procedures and boundary modules without importing external HTTP client packages.

## Problem Statement

Integrating heterogeneous external endpoints and internal worker nodes requires consistent outbound HTTP communication, reliable timeout handling, and secure credential injection without introducing redundant third-party transport libraries.

## Decision

1. MUST: Every outbound fetch invocation MUST include an AbortSignal timeout configuration to guarantee deterministic connection termination.

## Policy Block

- MUST Every outbound fetch invocation MUST include an AbortSignal timeout configuration to guarantee deterministic connection termination.

In scope:
- All outbound HTTP and HTTPS network requests originating from API procedures, routers, and internal integration modules.
- Direct communication with external services, webhook receivers, and internal worker endpoints.

Out of scope:
- Inbound API request routing and procedure definition.
- Direct database and cache operations managed through specialized datastore drivers.

Exceptions:
- EXC-26-001: An external service mandates a proprietary transport protocol or vendor-supplied SDK that encapsulates socket connections internally.

## Rationale

- Consistent reliance on the global fetch API across API routers provides a unified transport mechanism across all boundary integrations.
- Relying on built-in fetch avoids unnecessary dependency bloat, reduces supply-chain attack surfaces, and leverages standardized runtime capabilities.
- Mandating explicit abort signal timeouts and environment-based header configuration at each boundary point guarantees deterministic egress behavior.

## Consequences

Positive:
- Zero external package dependencies required for outbound network communications across service boundaries.
- Uniform request handling patterns leveraging standard Fetch API signatures and headers across all routing modules.
- Direct, auditable visibility into timeout limits and authorization credentials at each integration boundary.

Negative:
- Lack of a centralized transport wrapper requires explicit configuration of timeouts, headers, and error handling at each boundary call site.
- Network testing requires stubbing or intercepting the global fetch function rather than substituting an injected client instance.

## Alternatives

- Third-party HTTP client libraries (rejected)
  Rejected because: Introduces unnecessary bundle and runtime dependencies when the global fetch API natively satisfies timeout, header, and payload requirements.
  When valid: Environments running on legacy runtimes lacking global fetch support or requiring specialized transport adapters.
- Centralized HTTP client wrapper service (deferred)
  Rejected because: Current call sites exhibit heterogeneous payload structures, authentication schemes, and domain-specific error mapping that make a shared wrapper premature.
  When valid: When uniform cross-cutting retry, telemetry, and circuit-breaking policies must be managed across dozens of distinct remote endpoints.

## Risks

- Developers omitting timeout configurations on fetch calls, risking socket exhaustion under remote service latency.
  Mitigation: Enforce AbortSignal presence on every fetch invocation through automated linting and static analysis rules.
  Owner: API Engineering Team
- Inconsistent error parsing across varied third-party response formats leading to unhandled runtime rejections.
  Mitigation: Implement explicit status verification and schema parsing on outbound responses within boundary procedures.
  Owner: API Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct request headers using structured record mappings that sanitize or omit undefined keys prior to payload dispatch.
- Encapsulate recurring remote endpoint interactions within boundary helper modules when multiple procedures target identical services.

## Continuation Context


Verify commands:
- Discover the repository test runner from the root project manifest and execute integration tests covering outbound client boundaries.
- Discover the workspace static analysis script and verify all fetch call sites provide signal and authorization parameters.

Accept when:
- All outbound HTTP requests across API modules execute using the native fetch API with valid authorization and timeout signal options.
- Integration test suites mocking external endpoints pass without protocol or unhandled rejection errors.

## Enforcement

- Verified by: Automated static analysis rules verifying that outbound network requests use the global fetch API with configured abort signals.
- Verified by: Pull request peer reviews ensuring external credentials are injected solely from validated environment configurations.
- Violation handling: Pull requests introducing third-party HTTP transport dependencies or unconfigured fetch calls are flagged and blocked during validation checks.
- Exception process: Submit an architecture review request detailing the vendor-specific transport protocol limitations that necessitate an alternative client.