---
version: 2
name: fb4f7905-b0e4-4bcd-8cbf-adc142357492-adoption-of-slack-web-api-for-slack-platform-interactions-use-slack-web-client-instances-uniform
description: "Adoption of @slack/web-api for Slack Platform Interactions: Use Slack Web Client Instances Uniform"
metadata:
  actual:
    adr_id: fb4f7905-b0e4-4bcd-8cbf-adc142357492
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Use Slack Web Client Instances Uniform

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/slack/**`

## Context

- The server subsystem integrates with the Slack platform to handle slash commands, home tab rendering, event subscriptions, and interactive workflows.
- Slack platform operations require consistent request formatting, authentication token injection, and structured error classification across disparate route handlers.
- Adopting a standardized official client library prevents disparate and uncoordinated HTTP communication patterns across integration boundaries.

## Problem Statement

Interacting with the Slack platform across fragmented route handlers without a unified client library risks inconsistent error handling, duplicate payload serialization logic, and broken API contracts due to unmanaged REST calls. A unified client SDK adoption decision is required to ensure consistent, strongly typed, and maintainable communication with Slack platform endpoints.

## Decision

1. MUST: MUST use @slack/web-api client instances as the uniform interface for programmatic communication with Slack platform endpoints across all integration route handlers.

## Policy Block

- MUST MUST use @slack/web-api client instances as the uniform interface for programmatic communication with Slack platform endpoints across all integration route handlers.

In scope:
- Server-side route modules responsible for handling incoming Slack events, slash commands, interactive component callbacks, and home tab views.
- Internal services that initiate outbound messaging or view updates to the Slack platform.

Out of scope:
- Non-Slack third-party communication integrations.
- Low-level HTTP webhook signature verification layers executing prior to client route dispatch.

## Rationale

- The official client library provides typed interfaces and structured error objects that simplify interaction with complex Slack APIs like chat updates and view publication.
- The pattern is observed consistently across all Slack route handlers in the codebase, proving an established standard for platform integration.
- Adopting a single client SDK reduces maintenance surface area and centralizes error taxonomy handling across server integration endpoints.

## Consequences

Positive:
- Provides standardized, strongly typed method interfaces for platform operations including message updates and view publication.
- Reduces boilerplate payload construction and centralized platform-specific error handling.
- Maintains architectural consistency across commands, interactions, home views, and event handlers.

Negative:
- Couples message dispatch and view publication workflows to the release lifecycle and contract schema of the third-party client library.
- Increases dependency footprint across server deployment artifacts.
- Requires ongoing dependency governance to track upstream API deprecations and security advisories.

## Alternatives

- Direct HTTP fetch calls to Slack REST endpoints (rejected)
  Rejected because: Increases maintenance overhead by requiring manual HTTP header construction, manual serialization, and custom error mapping across every route handler.
  When valid: Acceptable only in resource-constrained environments where dependency bundle size cannot accommodate third-party SDK packages.
- Custom in-house Slack client abstraction layer (rejected)
  Rejected because: Introduces an unnecessary internal maintenance burden when an official client library with typed method contracts is already maintained by the platform vendor.
  When valid: Acceptable when custom middleware or uniform multi-platform messaging abstractions require vendor decoupling.

## Risks

- Upstream SDK version bumps may introduce breaking changes or alter method signatures.
  Mitigation: Rely on lock artifact resolution and run contract verification tests against mocked SDK responses during CI runs.
  Owner: Engineering Team
- Rate limits or platform outages from the Slack API can disrupt route execution.
  Mitigation: Wrap client calls in try-catch blocks with contextual logging and user-facing fallback notices.
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
- Configure client instances using workspace environment secrets resolved at runtime rather than hardcoding integration credentials.
- Structure message update calls to catch platform error codes and trigger appropriate fallback UI states or cleanup routines.
- Structure modal and home tab view payloads using dedicated view building functions before passing them to the publish API.

## Continuation Context


Verify commands:
- find_project_root_and_execute_typecheck
- find_project_root_and_execute_lint_check
- find_project_root_and_execute_unit_tests

Accept when:
- Static analysis confirms all Slack platform calls route through the official client SDK rather than raw HTTP requests.
- All Slack interaction and view publishing handlers conform to typed client method signatures without compilation errors.
- Integration test suites pass with mocked client responses reflecting expected platform payload structures.

## Enforcement

- Verified by: Continuous integration pipeline executing static typechecking and lint checks across all route handlers.
- Verified by: Automated dependency scanning to verify authorized external library imports.
- Verified by: Peer code review verifying that new Slack platform interactions utilize client methods rather than raw HTTP fetch invocations.
- Violation handling: Pull requests introducing unapproved HTTP clients or ad-hoc fetch calls to Slack endpoints will be blocked.
- Violation handling: Static analysis failures stemming from unverified client method signatures will prevent deployment pipeline progression.
- Exception process: Submit an architectural exception request documenting specific Slack API endpoints or streaming behaviors unsupported by the designated client SDK.
- Exception process: Obtain formal approval from backend lead engineers before committing custom transport layers.
- Exception process: Document any temporary transport overrides in dedicated architecture tracking tickets.