---
version: 2
name: 6a4c41ba-1da8-4844-a088-ecb169f0372d-openstatus-notification-base-environment-secrets-handling-developers-inspect-repository-lock-resolution-artifact
description: "@openstatus/notification-base Environment Secrets Handling: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 6a4c41ba-1da8-4844-a088-ecb169f0372d
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Developers Inspect Repository Lock Resolution Artifact

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/telegram`

Applies to:
- `src/**`

## Context

- The notification integration subsystem connects to external services using credentials passed via runtime environment variables.
- Direct invocation of outbound HTTP requests interpolates configuration parameters derived from process environment state.
- Input payloads are parsed and validated prior to external notification dispatch using structured schemas.

## Problem Statement

External notification dispatch requires authenticating against third-party service endpoints without exposing sensitive credentials in transit or coupling notification adapters to external secret management infrastructure.

## Decision

1. MUST: Developers MUST inspect the repository lock or resolution artifact to confirm the exact resolved dependency versions before implementing or updating notification client integrations.

## Policy Block

- MUST Developers MUST inspect the repository lock or resolution artifact to confirm the exact resolved dependency versions before implementing or updating notification client integrations.

In scope:
- Notification adapters and dispatch modules integrating with external messaging services
- Direct runtime credential resolution for outbound notification requests

Out of scope:
- Client-side browser applications and user interface components
- Internal inter-service authentication within the local network

## Rationale

- Reading secrets directly from process environment variables avoids introducing runtime overhead from external secret retrieval services during notification delivery.
- Enforcing payload schema validation prior to client invocation prevents malformed requests and unauthenticated execution paths.
- Centralizing credential resolution at the runtime boundary allows containerized deployments to inject secrets without baking credentials into application artifacts.

## Consequences

Positive:
- Simplifies credential configuration and deployment across isolated execution environments.
- Eliminates hardcoded credentials and prevents secret persistence within application source repositories.
- Ensures consistent payload validation prior to initiating external network transactions.

Negative:
- Process environment variables remain accessible to all code running within the same execution process.
- Passing credentials in uniform resource locators risks exposing secrets in upstream access logs and diagnostic telemetry.

## Alternatives

- Centralized Secret Management Service SDK (rejected)
  Rejected because: Introduces additional network hops, latency, and operational complexity for lightweight notification dispatch workers.
  When valid: When dynamic secret rotation, fine-grained access audits, or multi-tenant credential isolation are mandatory requirements.
- Filesystem Secret Volume Mounting (rejected)
  Rejected because: Requires persistent filesystem access and complicates stateless serverless or containerized deployment runtimes.
  When valid: When operating in dedicated container orchestration environments with native volume projection support.

## Risks

- Exposure of secret tokens in server access logs when interpolated directly into request URLs.
  Mitigation: Configure network client logging filters and transition outbound clients to header-based authentication where feasible.
  Owner: Engineering Team
- Missing environment variables causing runtime dispatch failures at call time.
  Mitigation: Validate the presence of required environment variables during service bootstrap.
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
- Verify that all required environment variables are defined in the runtime deployment configuration prior to service initialization.
- Ensure outbound request builders sanitize sensitive parameters before emitting diagnostic traces.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and test validation suites to verify secret handling and input schema compliance.
- Execute the repository code scanner to ensure no hardcoded secrets or exposed tokens exist in source files.

Accept when:
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

## Enforcement

- Verified by: Automated continuous integration checks executing static analysis and test suites on pull requests.
- Verified by: Peer code reviews verifying secure environment variable access and credential transmission patterns.
- Violation handling: Pull requests violating secret handling or schema validation rules will be blocked from merging until corrected.
- Exception process: Exceptions must be documented with an architectural assessment and approved by the security lead.