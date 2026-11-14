---
version: 2
name: 29c08eda-8418-4181-bbc7-08869efcdb24-adoption-of-next-auth-for-authentication-and-session-management-applications-requiring-user-identity-session-management
description: "Adoption of next-auth for Authentication and Session Management: Applications Requiring User Identity Session Management"
metadata:
  actual:
    adr_id: 29c08eda-8418-4181-bbc7-08869efcdb24
    status: accepted
    candidate_group_id: 3cc28c39-f695-45e8-bfe2-411e18bbbce5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of next-auth for Authentication and Session Management: Applications Requiring User Identity Session Management

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/lib/auth/**`

## Context

- Multi-tenant and domain-aware web applications require unified session handling and secure identity verification across localized routes.
- Dispersed custom authentication implementations increase vulnerability risks and maintenance complexity across server actions and API handlers.
- Centralizing authentication logic around next-auth allows consistent session resolution, header propagation, and user metadata synchronization.

## Problem Statement

Without a standardized authentication framework, disparate application endpoints and server actions risk implementing redundant, inconsistent, or insecure session verification and user authentication routines.

## Decision

1. MUST: Applications requiring user identity, session management, or authentication action handling MUST use next-auth as the standard authentication framework.

## Policy Block

- MUST Applications requiring user identity, session management, or authentication action handling MUST use next-auth as the standard authentication framework.

In scope:
- Application route handlers, server actions, and library modules managing user authentication or session verification

Out of scope:
- Public stateless endpoints and unauthenticated health check routes that do not inspect user identity

Exceptions:
- EXC-20-001: Stateless webhook endpoints validating signatures via provider-specific cryptographic signatures rather than user sessions

## Rationale

- Adopting next-auth standardizes authentication workflows and session persistence across application boundaries while minimizing boilerplate.
- Encapsulating next-auth configuration within a single library module ensures uniform header inspection and database viewer synchronization.
- Delegating session lifecycle to an established ecosystem framework reduces security liability compared to custom session token management.

## Consequences

Positive:
- Unified authentication logic and session resolution across all localized application routes and server actions.
- Reduced security vulnerability footprint through proven identity management and session token validation.
- Consistent synchronization of user activity and viewer database records across authentication events.

Negative:
- Tight coupling of application authentication flows to the next-auth library API surface.
- Architectural dependency on next-auth configuration conventions and adapter compatibility for database schemas.

## Alternatives

- Custom bespoke JWT and cookie session management implementation (rejected)
  Rejected because: Custom session handling introduces substantial security risks, ongoing maintenance overhead, and vulnerability to token management flaws.
  When valid: Environments where external dependencies are strictly prohibited and formal cryptographic audits are conducted internally.
- Third-party hosted identity provider integration using proprietary client SDKs (rejected)
  Rejected because: External managed identity providers introduce vendor lock-in, external network latency on session resolution, and recurring per-user infrastructure costs.
  When valid: Applications requiring enterprise federated single sign-on with multi-organization compliance requirements not supported by self-hosted schemas.

## Risks

- Major version transitions in next-auth may alter adapter interfaces or session callback signatures.
  Mitigation: Isolate next-auth dependencies within the centralized authentication library module so downstream actions consume internal abstractions.
  Owner: Engineering Team
- Misconfiguration of forwarded proto or host headers could cause session cookie validation failures in proxied domain environments.
  Mitigation: Validate forwarded header extraction logic in authentication library initialization tests across staging deployments.
  Owner: Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Encapsulate next-auth handlers, session callbacks, and database adapter connections inside a single internal library module, exporting only high-level session resolution and authentication primitives.
- When invoking authentication routines from application actions, ensure error states are caught and logged without exposing internal session tokens or raw authentication errors to the client.

## Continuation Context


Verify commands:
- manifest=$(find . -maxdepth 3 -name "*manifest*" -o -name "*package*" | head -n 1); [ -n "$manifest" ] && grep -q "next-auth" "$manifest" && echo "next-auth dependency verified in manifest"
- grep -rn "next-auth" . --exclude-dir=".*" 2>/dev/null | grep -q "next-auth" && echo "next-auth usage present"
- sh -c 'run_tests=$(find . -maxdepth 2 -name "*test*" -o -name "*spec*" | head -n 1); [ -n "$run_tests" ] && echo "Run discovered project test runner on authentication test suites"'

Accept when:
- The project dependency manifest and lock artifact both resolve next-auth to an active, locked version.
- Authentication routes and server actions route session validation through the centralized authentication module.
- Automated test suites discovered in the repository pass without authentication or type-check regressions.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository test and lint discovery scripts on pull requests
- Verified by: Peer architectural code review verifying that server actions consume the centralized authentication module instead of direct custom session handling
- Violation handling: Pull requests introducing direct custom session parsing or bypassing the centralized authentication module are blocked from merging until corrected.
- Exception process: Exceptions must be submitted via architectural review request and approved by the Security Lead and Principal Architect.