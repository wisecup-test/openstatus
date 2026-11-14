---
version: 2
name: f1903a4e-0017-42c0-a2b3-32c4328abf78-adoption-of-openstatus-services-agent-tools-for-centralized-agent-tool-definitions-protocol-adapters-returning-structured-execution-results
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Protocol Adapters Returning Structured Execution Results"
metadata:
  actual:
    adr_id: f1903a4e-0017-42c0-a2b3-32c4328abf78
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Protocol Adapters Returning Structured Execution Results

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Server applications expose domain capabilities across multiple external interfaces including chat platforms and protocol-specific tool servers.
- Defining tool schemas and business operations independently across multiple route handlers leads to schema drift and duplicated validation logic.
- A shared internal module structure centralizes tool definitions, input schemas, and output schemas for consumption by disparate runtime runners.

## Problem Statement

Exposing domain operational tools across heterogeneous protocols and chat integrations risks contract drift, inconsistent input validation, and fragmented maintenance when tool definitions are coupled directly to transport routes.

## Decision

1. MUST: Protocol adapters returning structured execution results MUST validate response payloads against the tool outputSchema contract before dispatching responses.

## Policy Block

- MUST Protocol adapters returning structured execution results MUST validate response payloads against the tool outputSchema contract before dispatching responses.

In scope:
- Route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

Out of scope:
- Internal service-to-service communication paths and background batch routines that do not interface with external agent protocols.

## Rationale

- Adopting @openstatus/services/agent-tools centralizes the specification of operational capabilities into a single reusable internal module.
- Enforcing inputSchema and outputSchema parsing across all transport adapters prevents malformed payloads from executing domain actions.
- Separating core tool logic from channel-specific presenters allows presentation formats to evolve without altering core tool contracts.

## Consequences

Positive:
- Establishes a single authoritative source for domain tool capabilities, inputs, and outputs across all integration channels.
- Guarantees uniform schema validation and error handling across distinct execution protocols.
- Simplifies testing of domain operations independently from transport-specific routing logic.

Negative:
- Couples multiple transport channels to the release cycle and dependency graph of a shared internal library.
- Requires adapter layers to translate shared schemas into transport-specific response structures.

## Alternatives

- Inline tool definitions and schema validation directly within transport-specific route handlers (rejected)
  Rejected because: Causes schema drift, duplicates validation logic across endpoints, and increases maintenance costs across transport boundaries.
  When valid: Isolated single-endpoint proof-of-concept implementations with no planned secondary integration channels.
- Independent protocol-specific tool packages maintained separately for each integration channel (rejected)
  Rejected because: Creates synchronization overhead and redundant implementations of identical domain business operations.
  When valid: Scenarios where different external consumers require fundamentally incompatible capabilities and security boundaries.

## Risks

- Breaking changes in shared tool schemas can propagate failures across multiple external integration channels simultaneously.
  Mitigation: Enforce strict backward compatibility and comprehensive regression test suites across all protocol adapters during continuous integration.
  Owner: engineering team
- Repetitive schema parsing across high-volume tool execution flows could introduce latency overhead.
  Mitigation: Compile and reuse parsed schema validators within registry adapters to avoid repetitive construction costs.
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
- When exposing new domain capabilities, define the tool specifications and associated validation schemas in the shared services library prior to implementing integration adapters.
- Ensure transport adapters register tool handlers into in-memory dispatch registries and delegate input parsing strictly to the declared schemas.

## Continuation Context


Verify commands:
- Discover the project workspace configuration and execute the static analysis script to verify that all agent tool imports resolve to @openstatus/services/agent-tools.
- Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools.

Accept when:
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

## Enforcement

- Verified by: Continuous integration pipelines executing static import verification and lint rules.
- Verified by: Automated unit and integration test suites validating schema compliance.
- Verified by: Mandatory peer code review for all changes to integration routes and adapters.
- Violation handling: Pull requests containing local tool declarations or bypassing centralized schemas will fail automated checks and be blocked from merging.
- Exception process: Exceptions require formal architectural review and documented justification submitted to the systems architecture group.