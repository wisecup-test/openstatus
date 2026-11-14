---
version: 2
name: ec0798fc-357a-41d4-aa20-433d1edf700e-twilio-messages-api-direct-integration-notification-payloads-validated-against-designated-schema
description: "Twilio Messages API Direct Integration: Notification Payloads Validated Against Designated Schema"
metadata:
  actual:
    adr_id: ec0798fc-357a-41d4-aa20-433d1edf700e
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Notification Payloads Validated Against Designated Schema

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/twillio-sms`

Applies to:
- `src/**`

## Context

- Outbound SMS delivery requires connecting notification dispatchers to external messaging gateway infrastructure
- Direct invocation of the Twilio Messages API via standard HTTP clients avoids bundling full vendor client libraries into lightweight notification packages
- The pattern is currently observed in an isolated notification integration file, reflecting localized implementation rather than a repository-wide standard

## Problem Statement

Notification delivery services require reliable transmission of status alerts to external SMS recipients without introducing heavy third-party SDK dependencies, yet isolated single-file implementations risk configuration drift and lack cross-module consistency.

## Decision

1. MUST: Notification payloads MUST be validated against the designated schema before constructing outbound external API requests.

## Policy Block

- MUST Notification payloads MUST be validated against the designated schema before constructing outbound external API requests.

In scope:
- Outbound SMS notification dispatch modules interacting with external telephony providers

Out of scope:
- Internal service-to-service communication mechanisms
- Non-SMS notification delivery channels

Exceptions:
- EXC-26-001: A dedicated vendor SDK is formally approved for multi-channel communication requirements

## Rationale

- Direct HTTP communication with the Twilio Messages API minimizes runtime dependency footprint by avoiding provider-specific SDK distribution overhead
- Schema validation prior to external transmission guarantees payload conformity before network egress
- Because evidence is confined to a single file, the practice remains candidate-level and lacks broader architectural adoption across the codebase

## Consequences

Positive:
- Reduces external dependency overhead by utilizing standard HTTP interfaces for Twilio Messages API access
- Ensures data validation occurs at service boundaries prior to dispatching external network requests
- Decouples message generation from proprietary client runtime libraries

Negative:
- Increases maintenance burden by requiring manual maintenance of HTTP request headers, parameter serialization, and error parsing
- Lacks centralized retry, rate-limiting, and backoff handling across differing notification providers

## Alternatives

- Vendor-provided SDK adoption (rejected)
  Rejected because: Introduces unnecessary package weight and transitive dependencies for basic SMS transmission
  When valid: Advanced messaging features such as multi-region failover or batch messaging are required
- Centralized internal notification gateway service (deferred)
  Rejected because: Current notification volume and complexity do not warrant dedicated service infrastructure
  When valid: Multiple internal services require independent access to SMS dispatch capabilities

## Risks

- Changes to external provider HTTP contracts could disrupt message delivery without compile-time warnings
  Mitigation: Enforce contract and schema validation testing against mock provider responses in automated verification suites
  Owner: Engineering team
- Direct HTTP implementations across multiple packages may lead to inconsistent retry and timeout handling
  Mitigation: Establish shared notification base utilities for HTTP boundary interactions
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure outbound authorization headers and credentials are loaded exclusively through validated environment configurations
- Construct URL-encoded request bodies using standard key-value serialization matching external provider parameter specifications

## Continuation Context


Verify commands:
- Discover and run the project static analysis and test validation suites from the repository root
- Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications

Accept when:
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

## Enforcement

- Verified by: Automated continuous integration pipeline checks
- Verified by: Peer code review for outbound external client integrations
- Violation handling: Pull requests bypassing schema validation or embedding hardcoded external endpoints are blocked from merging
- Exception process: Exceptions require submission of an architectural waiver approved by lead maintainers