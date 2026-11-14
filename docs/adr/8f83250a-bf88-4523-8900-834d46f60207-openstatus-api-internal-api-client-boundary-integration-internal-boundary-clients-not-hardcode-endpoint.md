---
version: 2
name: 8f83250a-bf88-4523-8900-834d46f60207-openstatus-api-internal-api-client-boundary-integration-internal-boundary-clients-not-hardcode-endpoint
description: "@openstatus/api Internal API Client Boundary Integration: Internal Boundary Clients Not Hardcode Endpoint"
metadata:
  actual:
    adr_id: 8f83250a-bf88-4523-8900-834d46f60207
    status: accepted
    candidate_group_id: 1c383329-dd15-4c54-bd3d-525d24ca03a5
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/onboarding/**
      - src/lib/trpc/**
---

# @openstatus/api Internal API Client Boundary Integration: Internal Boundary Clients Not Hardcode Endpoint

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/onboarding/**`
- `src/lib/trpc/**`

## Context

- The dashboard application requires communication with internal services and API route handlers across heterogeneous runtime environments.
- Server-rendered modules and data loaders must forward user authentication session tokens to internal API endpoints without leaking credentials or duplicating session resolution logic.
- Client-side interactive workflows require real-time streaming updates from internal service checks with client-driven cancellation to conserve network and compute resources.
- Direct invocations of internal API boundaries necessitate standardized transport configuration, header propagation, and error boundary handling.

## Problem Statement

Interacting with internal APIs across server and client boundaries presents distinct lifecycle and security challenges. Server-side callers require authenticated session forwarding to maintain identity context when querying internal API procedures, while client-side callers consuming real-time streaming endpoints require explicit lifecycle management and abort signaling to prevent resource exhaustion and hanging connections.

## Decision

1. MUST_NOT: Internal API boundary clients MUST NOT hardcode endpoint URLs or bypass standardized transport header forwarders when communicating across service boundaries.

## Policy Block

- MUST_NOT Internal API boundary clients MUST NOT hardcode endpoint URLs or bypass standardized transport header forwarders when communicating across service boundaries.

In scope:
- Internal API client modules operating within server runtime environments requiring session credential forwarding.
- Client-side UI components and hooks communicating with internal streaming or standard HTTP route handlers.

Out of scope:
- External third-party API integrations that do not share internal authentication or protocol contracts.
- Public static asset requests that do not require session context or lifecycle cancellation.

Exceptions:
- EXC-26-001: A background batch synchronization task operates without an active user session context.

## Rationale

- Explicit server-side cookie header forwarding ensures internal API calls respect user authorization boundaries without re-authenticating on every internal call.
- Encapsulating streaming endpoint interactions with AbortController prevents resource leaks, race conditions, and unmounted component state mutation.
- Maintaining environment-specific client boundary configurations prevents server credential leakage into browser client bundles.

## Consequences

Positive:
- Consistent propagation of session authentication across server-side internal service boundaries.
- Deterministic resource reclamation and request cancellation for streaming client operations.
- Clear architectural separation between server-only RPC data fetching and client-side streaming hooks.

Negative:
- Increased boilerplate for custom transport configuration and header forwarding logic across boundaries.
- Requirement for defensive parsing logic across streaming consumers to protect against malformed chunks.

## Alternatives

- Direct database access from server components bypassing internal API boundaries (rejected)
  Rejected because: Bypassing internal API boundaries couples UI rendering tightly to database schemas and fragments domain validation logic.
  When valid: Valid only in standalone prototypes without distributed service architectures.
- Unified WebSocket connection for all internal streaming interactions (rejected)
  Rejected because: WebSockets introduce persistent connection state and infrastructure overhead unnecessary for short-lived streaming checks.
  When valid: Valid for bidirectional, high-frequency collaborative editing or gaming workloads.

## Risks

- Session cookie forwarding failure resulting in unauthorized internal API rejections.
  Mitigation: Enforce automated integration testing covering authenticated server-side caller workflows and credential forwarding.
  Owner: Platform Engineering
- Uncaught exceptions during streaming JSON chunk parsing causing client UI crashes.
  Mitigation: Mandate defensive try-catch wrappers and user-facing error notifications around stream line parsing.
  Owner: Frontend Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure server-side internal API clients are isolated within server-only execution environments to prevent bundling server credentials into client-facing artifacts.
- Construct client-side streaming hooks with cleanup callbacks that invoke abort methods on active controller instances upon component unmount.
- Verify that content-type headers and authentication cookie headers are properly aligned between client fetch options and internal API expectations.

## Continuation Context


Verify commands:
- echo "Discover and execute project test suite for internal API client boundaries"
- echo "Discover and execute project typecheck and linting suite for boundary contracts"
- echo "Discover and execute project integration test suite verifying session cookie propagation and streaming cancellation"

Accept when:
- All internal API client calls successfully execute and pass automated integration test suites without credential omission errors.
- Client-side streaming hooks successfully terminate active network requests upon component unmount without uncaught runtime errors.
- Repository type checking and linting pass with zero boundary contract violations.

## Enforcement

- Verified by: Automated continuous integration test suites verifying client boundary contracts and credential forwarding.
- Verified by: Static analysis and lint rules restricting server-only modules from client bundle imports.
- Verified by: Peer code review for all internal API integration endpoints and client transport configurations.
- Violation handling: Continuous integration pipeline blocks pull requests with failing integration tests or boundary type errors.
- Violation handling: Code review rejections for unhandled stream cancellations or hardcoded credentials in client modules.
- Exception process: Submit an architectural deviation request detailing why standard boundary adapters cannot fulfill requirements.
- Exception process: Obtain approval from the platform engineering lead and document the compensating controls.