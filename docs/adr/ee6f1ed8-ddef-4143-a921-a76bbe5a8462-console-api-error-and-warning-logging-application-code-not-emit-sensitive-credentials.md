---
version: 2
name: ee6f1ed8-ddef-4143-a921-a76bbe5a8462-console-api-error-and-warning-logging-application-code-not-emit-sensitive-credentials
description: "Console API Error and Warning Logging: Application Code Not Emit Sensitive Credentials"
metadata:
  actual:
    adr_id: ee6f1ed8-ddef-4143-a921-a76bbe5a8462
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Application Code Not Emit Sensitive Credentials

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/**`

## Context

- Client-side form handlers, user interface event interactions, and routing proxy handlers require a lightweight mechanism to report unexpected state transitions and caught promise rejections.
- The codebase invokes runtime console.error and console.warn calls across client components and proxy boundaries rather than routing diagnostics through a dedicated logging abstraction.
- Unhandled validation failures and invariant breaches are recorded directly to stdout or browser developer consoles during execution.

## Problem Statement

Direct invocation of runtime console.error and console.warn across user interface components and proxy handlers creates inconsistent diagnostic output without centralized log leveling, telemetry forwarding, or structured payload formatting.

## Decision

1. MUST_NOT: Application code MUST NOT emit sensitive credentials, session tokens, or unredacted personal identifiers into diagnostic calls through console.error or console.warn.

## Policy Block

- MUST_NOT Application code MUST NOT emit sensitive credentials, session tokens, or unredacted personal identifiers into diagnostic calls through console.error or console.warn.

In scope:
- Client-side interactive user interface components, form mutation handlers, and edge proxy handlers capturing operational anomalies.
- Unhandled promise rejections and schema parsing discrepancies detected during client runtime execution.

Out of scope:
- Shared backend service modules and database transaction contexts that require structured telemetry and persistent log retention.
- Performance monitoring, metric emission, and distributed tracing systems.

## Rationale

- Runtime console calls require zero bundle overhead, zero configuration, and execute identically across browser client environments and runtime edge handlers.
- Direct logging of caught exceptions during form submissions and validation checks ensures developer visibility during local development and browser debugging.
- The absence of a dedicated logging facade indicates that current observability requirements for client interfaces rely on browser host inspection and proxy output.

## Consequences

Positive:
- Zero bundle overhead and zero external runtime dependencies introduced for basic diagnostic output.
- Immediate visibility of client-side execution anomalies in standard developer tooling and serverless proxy output streams.
- Uniform diagnostic invocation style across user interface mutation handlers and edge proxy logic.

Negative:
- Lack of structured log schemas, centralized transport, or automated indexing for production client-side issues.
- Risk of sensitive payload data leakage if unredacted request parameters or error objects are logged.
- Inability to dynamically toggle log levels or filter diagnostics by severity across deployment environments.

## Alternatives

- Adoption of a centralized third-party structured logging client library (rejected)
  Rejected because: Increases client bundle weight and introduces additional runtime dependency overhead without existing infrastructure to ingest structured client streams.
  When valid: When enterprise compliance or centralized telemetry aggregation requires streaming all client and proxy exceptions to an external log storage backend.
- Silent error suppression without console emission (rejected)
  Rejected because: Suppresses diagnostic context, preventing developers and operators from diagnosing client-side form failures or proxy invariant breaches.
  When valid: When errors represent fully recoverable transient states with dedicated user interface fallback states that require no debugging context.

## Risks

- Accidental leakage of sensitive user data or authentication tokens through unredacted console calls in production builds.
  Mitigation: Enforce linting rules and code review policies prohibiting the logging of full request payloads or session tokens.
  Owner: Engineering Team
- Loss of diagnostic visibility for critical client-side failures that occur in production environments without active developer consoles.
  Mitigation: Evaluate remote error tracking integrations to capture unhandled exceptions automatically.
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
- Audit all diagnostic call sites to ensure error objects or string literals do not incorporate sensitive authentication identifiers or private user tokens.
- Standardize warning and error string prefixes to simplify filtering within runtime output streams.

## Continuation Context


Verify commands:
- Discover the project static analysis and linting scripts from the repository configuration and run them to identify unauthorized console usage patterns.
- Discover and execute the automated test suites defined in the repository manifest to verify that error handling blocks behave correctly under failure conditions.

Accept when:
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline scanning for disallowed console call signatures.
- Verified by: Peer code review for all pull requests modifying error handling or logging call sites.
- Violation handling: Pull requests containing unapproved console logging patterns or sensitive data leaks are blocked from merging.
- Violation handling: Flagged violations must be remediated to sanitize payloads or adhere to logging constraints.
- Exception process: Exceptions must be documented in the corresponding pull request and approved by a technical lead before merging.