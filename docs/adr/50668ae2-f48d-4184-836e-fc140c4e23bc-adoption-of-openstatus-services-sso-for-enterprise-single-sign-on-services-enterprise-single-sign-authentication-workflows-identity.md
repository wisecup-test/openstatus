---
version: 2
name: 50668ae2-f48d-4184-836e-fc140c4e23bc-adoption-of-openstatus-services-sso-for-enterprise-single-sign-on-services-enterprise-single-sign-authentication-workflows-identity
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Enterprise Single Sign Authentication Workflows Identity"
metadata:
  actual:
    adr_id: 50668ae2-f48d-4184-836e-fc140c4e23bc
    status: accepted
    candidate_group_id: d98bb90f-25e2-42e0-bf3f-669cc5c4be85
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Enterprise Single Sign Authentication Workflows Identity

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`
- `src/lib/auth/**`

## Context

- Enterprise single sign-on workflows span multiple ingress surfaces including server actions, identity provider redirect routes, and provider webhook endpoints.
- Dispersing identity provider communication and signature verification logic across route handlers risks configuration drift and inconsistent session handling.
- The repository utilizes internal workspace service modules to encapsulate domain boundaries across application tiers.

## Problem Statement

Decentralized implementations of enterprise single sign-on logic across route handlers and UI actions lead to duplicated identity provider communication, inconsistent signature validation, and divergent session cookie configurations, creating maintenance overhead and potential security vulnerabilities.

## Decision

1. MUST: All enterprise single sign-on authentication workflows, identity provider routing, and identity provider webhook handling MUST encapsulate domain operations within @openstatus/services/sso.

## Policy Block

- MUST All enterprise single sign-on authentication workflows, identity provider routing, and identity provider webhook handling MUST encapsulate domain operations within @openstatus/services/sso.

In scope:
- All server actions, route handlers, and webhook receivers implementing enterprise authentication or single sign-on.
- Internal service modules managing directory synchronization and identity provider interactions.

Out of scope:
- Standard credentials-based or email magic link authentication mechanisms that do not involve enterprise identity providers.
- Public, non-authenticated informational endpoints and landing pages.

## Rationale

- Centralizing enterprise authentication logic within @openstatus/services/sso ensures consistent enforcement of cookie security, signature validation, and organization lookup rules.
- Encapsulating provider-specific details inside a domain module decouples web request handling from identity provider protocol changes.
- Evidence across multiple application endpoints demonstrates an established architecture of routing authentication domain operations through this dedicated module.

## Consequences

Positive:
- Establishes a single source of truth for identity provider interactions, cookie generation, and directory events.
- Reduces defect surface by standardizing cookie security attributes and signature validation rules.
- Simplifies unit and integration testing by isolating domain logic from framework request contexts.

Negative:
- Application endpoints become tightly coupled to the internal service API contract defined by @openstatus/services/sso.
- Modifications to single sign-on interfaces require synchronized updates across authentication actions and webhook routes.

## Alternatives

- Direct inline integration with third-party identity providers within individual route handlers and server actions (rejected)
  Rejected because: Causes duplication of authentication workflows, inconsistent session cookie configuration, and fragmented error handling across endpoints.
  When valid: Only valid in single-endpoint prototypes where enterprise authentication is not shared across application surfaces.
- Externalizing single sign-on logic entirely to an independent microservice over HTTP (rejected)
  Rejected because: Introduces operational network overhead and deployment complexity unnecessary for a unified monorepo architecture.
  When valid: Valid when distinct operational domains require independent horizontal scaling and decoupled release cycles.

## Risks

- A failure or regression in @openstatus/services/sso impacts all enterprise authentication entry points simultaneously.
  Mitigation: Maintain comprehensive test coverage for all exported service functions and provide isolated mock utilities for consumer tests.
  Owner: engineering team
- Breaking interface changes within the service package require coordinated migrations across multiple application surfaces.
  Mitigation: Establish clear contract definitions and backward compatibility checks for shared service interfaces.
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
- Ensure session and state cookies emitted during single sign-on handshakes enforce HTTP-only, secure, and restricted cross-site policies.
- Encapsulate identity provider signature verification routines completely within the service boundary before payload consumption.

## Continuation Context


Verify commands:
- Discover the repository test runner from the build manifest and execute test suites covering identity provider and authentication modules.
- Discover the repository linter and static analysis configuration to verify module boundary compliance across authentication routes.

Accept when:
- Static analysis confirms all single sign-on operations, webhook handlers, and authentication actions route through @openstatus/services/sso.
- Automated test suites pass without regressions across enterprise authentication and identity provider integration paths.
- All dependencies resolve to explicit entries defined within the repository lock artifact.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites and static analysis tools.
- Verified by: Peer review during pull requests to verify module boundary compliance.
- Violation handling: Pull requests introducing bypassed identity provider logic or direct external client calls must be blocked.
- Violation handling: Violating implementations must be refactored to delegate domain operations through @openstatus/services/sso.
- Exception process: Submit an architecture exception request detailing the necessity of bypassing @openstatus/services/sso.
- Exception process: Obtain review and sign-off from the architecture review group prior to merging non-compliant workflows.