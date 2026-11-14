---
version: 2
name: 108692af-3238-44b7-b094-3565271cd08c-adoption-of-the-ai-library-for-conversational-interfaces-and-agent-workflows-conversational-components-not-invent-custom-message
description: "Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Conversational Components Not Invent Custom Message"
metadata:
  actual:
    adr_id: 108692af-3238-44b7-b094-3565271cd08c
    status: accepted
    candidate_group_id: 728cb320-99d6-4631-95b6-9015d5af629e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/api/chat/**
      - src/components/chat/**
      - src/lib/agent-tools/**
---

# Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Conversational Components Not Invent Custom Message

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/api/chat/**`
- `src/components/chat/**`
- `src/lib/agent-tools/**`

## Context

- The system provides conversational AI capabilities requiring coordinated bidirectional communication, agent tool orchestration, and dynamic message stream rendering.
- Disparate implementations of conversational protocols across client and server layers introduce maintenance overhead and serialization drift.
- The project adopts the ai library as the unified foundational abstraction for message schemas, agent tool execution, and conversational state handling across frontend and backend boundaries.

## Problem Statement

Without a unified library for conversational AI primitives, application modules must independently define message representations, manage streaming lifecycles, and handle tool call orchestration. This leads to redundant protocol definitions, inconsistent validation of chat payloads, and brittle integration between backend agent tool execution and client-side conversational rendering.

## Decision

1. MUST_NOT: Conversational components MUST NOT invent custom message schema shapes or bespoke streaming protocols that bypass the data structures established by the ai library.

## Policy Block

- MUST_NOT Conversational components MUST NOT invent custom message schema shapes or bespoke streaming protocols that bypass the data structures established by the ai library.

In scope:
- All server-side endpoints orchestrating conversational chat interactions and streaming responses.
- All agent tool adapters defining executable actions and input parameter schemas.
- All client-side hooks, state managers, and presentation components rendering conversational messages.

Out of scope:
- Standard request-response API endpoints unrelated to conversational streaming or agent features.
- Static content views and administrative interfaces that do not interact with chat sessions or agent execution.

## Rationale

- Standardizing on the ai library provides a cohesive interface across server-side agent execution and client-side conversational rendering.
- Observed implementation across five distinct files demonstrates consistent reliance on the library for tool adaptation, route handling, session querying, and message rendering.
- Decouples business domain services from low-level conversational stream orchestration while maintaining end-to-end schema integrity.

## Consequences

Positive:
- Eliminates protocol discrepancies between conversational server endpoints and client presentation components.
- Enables structured schema validation for all agent tool inputs and conversational message payloads.
- Simplifies conversational state synchronization through unified message part models across tiers.

Negative:
- Couples conversational workflows and tool adapters to the API contracts of the selected library.
- Upgrades to the library necessitate coordinated compatibility verification across both backend handlers and frontend UI components.

## Alternatives

- Bespoke streaming protocol and custom chat message serialization (rejected)
  Rejected because: Increases maintenance overhead and bug surface area by requiring custom stream lifecycle management and deserialization routines.
  When valid: When operating under specialized transport constraints where third-party streaming libraries cannot be deployed.
- Decoupled library selection where backend routes and frontend presentation consume distinct conversational frameworks (rejected)
  Rejected because: Introduces schema drift and impedance mismatch between server-generated message chunks and client-side rendering hooks.
  When valid: When client and server run on disparate platforms without common runtime language bindings.

## Risks

- Upstream changes to the core streaming or message interface could disrupt existing route handlers and client rendering pipelines.
  Mitigation: Lock library versions strictly and validate all API interactions against automated test suites prior to dependency upgrades.
  Owner: engineering team
- Improper input validation in agent tool adapters could allow malformed execution payloads.
  Mitigation: Enforce strict schema parsing on all tool parameters prior to invocation.
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
- Define agent tools by pairing domain service capabilities with schema definitions that validate incoming execution arguments.
- Handle streaming conversational persistence asynchronously to prevent storage failures from aborting active client chat sessions.

## Continuation Context


Verify commands:
- # Discover project verification scripts from the repository manifest and execute the test suite
- # Discover and run static analysis checks across modules consuming the conversational library

Accept when:
- Static type analysis and schema validations pass across all conversational endpoints and UI components.
- Automated tests verify that agent tool adapters successfully parse valid inputs and reject malformed schemas.
- Conversational interactions stream messages and persist session states without unhandled exceptions.

## Enforcement

- Verified by: Continuous integration pipeline running automated tests and static type checks.
- Verified by: Peer code review verifying adherence to library abstractions and schema validation rules.
- Violation handling: Pull requests containing unvalidated conversational inputs or bespoke streaming protocols are blocked.
- Violation handling: Violations must be refactored to align with the standardized library contracts prior to merging.
- Exception process: Exceptions require an architectural review outlining why the standard library cannot satisfy functional requirements.
- Exception process: Approved exceptions must document mitigation strategies for protocol drift and schema maintenance.