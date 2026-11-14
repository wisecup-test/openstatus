---
version: 2
name: 7809d048-491e-4c35-9827-370e68e9e45f-resend-client-adoption-for-transactional-email-delivery-transactional-email-dispatch-use-resend-client
description: "Resend Client Adoption for Transactional Email Delivery: Transactional Email Dispatch Use Resend Client"
metadata:
  actual:
    adr_id: 7809d048-491e-4c35-9827-370e68e9e45f
    status: accepted
    candidate_group_id: 06994a71-dc9f-48f1-80c5-a18094061812
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Client Adoption for Transactional Email Delivery: Transactional Email Dispatch Use Resend Client

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/emails`

Applies to:
- `src/**`

## Context

- The platform requires automated transactional communication capabilities, including onboarding follow-ups, workspace invitations, and user feedback requests.
- Operating and maintaining self-hosted mail transfer infrastructure incurs significant operational complexity, deliverability monitoring, and security maintenance burdens.
- Standardizing on a single delivery client across client wrappers and sending modules prevents divergent mail delivery implementations and simplifies error handling.

## Problem Statement

Managing disparate email dispatch mechanisms across application services risks inconsistent deliverability tracking, duplicated client authentication logic, and fragmented error handling across notification workflows.

## Decision

1. MUST: All transactional email dispatch MUST use the Resend client library through standardized transmission interfaces rather than custom mail transport protocols.

## Policy Block

- MUST All transactional email dispatch MUST use the Resend client library through standardized transmission interfaces rather than custom mail transport protocols.

In scope:
- Modules responsible for formatting and transmitting transactional notifications, user invitations, and engagement emails.
- Batch messaging workflows sending communication to multiple recipients simultaneously.

Out of scope:
- Internal inter-service messaging or queue-based event streaming that does not deliver outbound emails.
- Email template visual styling and component rendering logic decoupled from delivery transmission.

Exceptions:
- EXC-20-001: A third-party webhook or specialized integration requires direct HTTP transmission to an alternate messaging endpoint.

## Rationale

- Consolidating outbound communications under the Resend SDK provides consistent single and multi-recipient dispatch interfaces.
- Centralizing authentication and environment configuration through a dedicated client boundary minimizes secret exposure risks and unifies operational logging across delivery attempts.
- Batch dispatch endpoints reduce network round-trips when broadcasting notifications across workspace members or subscriber cohorts.

## Consequences

Positive:
- Standardized email delivery semantics across single and batch transmission patterns.
- Consistent error handling and diagnostic logging for all delivery failures.
- Reduced operational overhead by delegating deliverability, reputation management, and transport infrastructure to an external service.

Negative:
- Tight coupling to the Resend API contract and authentication model for outbound messaging.
- Network boundary dependency where service downtime or API rate limiting directly impacts message delivery.
- Financial costs scaling with outbound transactional email volume.

## Alternatives

- Self-hosted SMTP server infrastructure (rejected)
  Rejected because: Incurs substantial maintenance overhead, complex deliverability and IP reputation management, and lack of native batch REST interfaces.
  When valid: Strict air-gapped environments or regulatory mandates prohibiting third-party cloud mail processing.
- Generic HTTP client invocations without an official SDK wrapper (rejected)
  Rejected because: Requires manual maintenance of API payload structures, repetitive header construction, and custom retry logic across each sending module.
  When valid: Environments with severe package size constraints where adding external SDK dependencies is prohibited.

## Risks

- Third-party service outage or latency spikes during transactional email dispatch.
  Mitigation: Capture dispatch failures through structured error logging and decouple high-volume sending via background queues or resilient retry mechanisms.
  Owner: Platform Engineering
- Exhaustion of rate limits during bulk or multi-recipient batch sending.
  Mitigation: Utilize batch send interfaces to aggregate recipient dispatches into unified network payloads and enforce outbound pacing.
  Owner: Platform Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Instantiate email delivery clients using API keys sourced strictly from runtime environment configurations, never embedding credentials within client source files.
- When dispatching messages to multiple recipients, prefer batch sending interfaces over serial single-message dispatches to minimize network overhead.

## Continuation Context


Verify commands:
- Discover and run the project's dependency audit and integrity check scripts to verify the email client library matches the locked manifest version.
- Discover and run the project's static analysis and type checking suite to confirm email client imports and method invocations conform to client signatures.
- Discover and run the project's unit and integration test suite to validate mock email dispatch behaviors, error handling paths, and environment credential resolution.

Accept when:
- All static analysis and type checking suites pass without type mismatch errors on email client method invocations.
- The dependency resolution artifact confirms the email client package matches the approved locked version.
- Automated tests confirm error logging triggers upon simulated transmission failure without crashing the host process.

## Enforcement

- Verified by: Automated continuous integration pipeline running type checks, lint checks, and test suites.
- Verified by: Peer code review verifying outbound email implementations use approved client methods and avoid hardcoded secrets.
- Violation handling: Continuous integration failure blocking merge for any unapproved mail transport implementations or direct secret inclusion.
- Violation handling: Code review rejection requiring refactoring to use the standardized email client.
- Exception process: Formal architecture review submission outlining technical justification for alternative mail delivery mechanisms, approved by the engineering lead.