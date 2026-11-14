---
version: 2
name: 84ab19ac-0a48-4a7c-8719-11466cccfed6-next-server-library-module-adoption-before-implementing-updating-endpoints-utilizing-next
description: "next/server Library Module Adoption: Before Implementing Updating Endpoints Utilizing Next"
metadata:
  actual:
    adr_id: 84ab19ac-0a48-4a7c-8719-11466cccfed6
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Before Implementing Updating Endpoints Utilizing Next

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/checker/cron/**`

## Context

- Server-side HTTP routing and scheduled execution endpoints require standardized primitives for request handling and response dispatching.
- Multiple recurring task routes across varied execution intervals consistently import next/server alongside local task execution and monitoring utilities.
- Consolidating on next/server establishes a uniform boundary interface across all scheduled server-side endpoints.

## Problem Statement

Disparate or unstandardized server request handling creates fragmentation across scheduled background endpoints, increasing maintenance overhead and complicating telemetry integration.

## Decision

1. MUST: Before implementing or updating endpoints utilizing next/server, the consumer MUST inspect the repository dependency resolution lock artifact to determine the exact resolved version and verify that all invoked server module interfaces exist in that version.

## Policy Block

- MUST Before implementing or updating endpoints utilizing next/server, the consumer MUST inspect the repository dependency resolution lock artifact to determine the exact resolved version and verify that all invoked server module interfaces exist in that version.

In scope:
- Server-side HTTP route handler endpoints.
- Scheduled task execution routes requiring structured HTTP dispatch and response handling.

Out of scope:
- Client-side components and presentation rendering layers.
- Standalone utility modules that operate independently of HTTP request lifecycles.

## Rationale

- Adopting next/server standardizes HTTP request lifecycle and response abstractions across all server-side endpoints.
- Empirical evidence across all recurring task intervals demonstrates consistent usage of next/server alongside internal task orchestration and monitoring modules.
- Centralizing on a single server module interface minimizes protocol divergence and ensures cohesive telemetry and error propagation.

## Consequences

Positive:
- Uniform request and response handling across all scheduled invocation cadences.
- Simplified integration with internal task coordinators and telemetry utilities.
- Predictable server-side error handling and response formatting.

Negative:
- Direct coupling of server endpoints to next/server module interface contracts.
- Requirement to manage framework version compatibility and server runtime deprecations across all route handlers.

## Alternatives

- Custom HTTP Protocol Handlers (rejected)
  Rejected because: Increases boilerplate, lacks unified framework lifecycle management, and creates maintenance overhead.
  When valid: When running in standalone bare-runtime environments without server framework capabilities.
- Third-Party External Middleware Framework (rejected)
  Rejected because: Introduces redundant dependencies when next/server provides native request and response handling.
  When valid: When migrating services across heterogeneous multi-cloud hosting runtimes.

## Risks

- Upstream API contract changes or deprecations in next/server could impact scheduled endpoint stability.
  Mitigation: Lock version dependencies strictly and verify interface parity against documentation before adopting updates.
  Owner: Engineering Team
- Over-coupling endpoint business logic with next/server request objects reduces handler portability.
  Mitigation: Isolate domain logic into dedicated internal coordinators and maintain next/server purely as an edge routing boundary.
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
- Keep route handler functions focused on request validation, delegating actual task execution to internal modules.
- Ensure response payloads adhere to consistent structured representations for automated monitoring.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type checking verification scripts.
- Discover and execute automated test suites covering server route handlers.
- Inspect dependency resolution lock artifacts to verify server library module integrity.

Accept when:
- All server-side route handlers compile cleanly and adhere to next/server module interfaces.
- Automated tests covering endpoint dispatch and response formats pass without regression.

## Enforcement

- Verified by: Automated continuous integration checks verifying code compilation and lint standards.
- Verified by: Peer code review for all new and updated server route handler endpoints.
- Violation handling: Pull requests bypassing next/server or introducing unverified version dependencies are blocked from merging until corrected.
- Exception process: Architectural review and written approval from senior engineering leads documenting the justification for alternative transport interfaces.