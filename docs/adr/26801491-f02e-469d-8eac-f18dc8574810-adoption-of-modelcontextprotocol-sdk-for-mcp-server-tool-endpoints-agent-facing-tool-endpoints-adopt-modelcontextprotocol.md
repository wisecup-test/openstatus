---
version: 2
name: 26801491-f02e-469d-8eac-f18dc8574810-adoption-of-modelcontextprotocol-sdk-for-mcp-server-tool-endpoints-agent-facing-tool-endpoints-adopt-modelcontextprotocol
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Agent Facing Tool Endpoints Adopt Modelcontextprotocol"
metadata:
  actual:
    adr_id: 26801491-f02e-469d-8eac-f18dc8574810
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Agent Facing Tool Endpoints Adopt Modelcontextprotocol

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/mcp/tools/**`

## Context

- Server endpoints must expose operational domain capabilities to external agents using a standardized context and protocol interface.
- Multiple domain modules require a consistent integration pattern connecting domain services with the external protocol runtime.
- Internal service boundaries must remain decoupled from specific external protocol transport structures through dedicated registry adaptation.

## Problem Statement

Exposing domain operational capabilities directly to external AI agents risks divergent protocol bindings, inconsistent tool schema definitions, and tight coupling between domain business logic and agent runtime protocols across distinct operational areas.

## Decision

1. MUST: All agent-facing tool endpoints MUST adopt @modelcontextprotocol/sdk/server/mcp.js as the standardized interface boundary for defining and registering server tools.

## Policy Block

- MUST All agent-facing tool endpoints MUST adopt @modelcontextprotocol/sdk/server/mcp.js as the standardized interface boundary for defining and registering server tools.

In scope:
- Tool handler modules exposing server domain capabilities through the Model Context Protocol
- Modules integrating internal domain services with external agent execution environments

Out of scope:
- Internal domain service implementations that do not interface directly with external agents
- Standard HTTP REST or GraphQL API endpoints governed by alternate gateway conventions

## Rationale

- Static analysis across seven distinct operational tool modules demonstrates an established, recurring integration pattern adopting @modelcontextprotocol/sdk/server/mcp.js.
- Standardizing on @modelcontextprotocol/sdk guarantees compatibility with compliant client agents without requiring proprietary communication protocols.
- Enforcing mediation through internal service abstractions and registry adapters ensures that changes to the protocol SDK do not compromise core domain logic.

## Consequences

Positive:
- Provides uniform tool registration and contract definitions across all domain areas.
- Aligns server capabilities with the standardized Model Context Protocol specification.
- Isolates core business logic from agent communication mechanics through adapter mediation.

Negative:
- Introduces an external dependency on the Model Context Protocol SDK and its ongoing interface evolution.
- Requires maintaining adapter layers to map domain service structures into protocol-compatible payloads.

## Alternatives

- Custom JSON-RPC Tool Interface (rejected)
  Rejected because: Creating a proprietary tool communication protocol increases maintenance overhead and prevents interoperability with standard agent ecosystems.
  When valid: Valid only in isolated environments where external agent interoperability is prohibited and closed protocols are strictly mandated.
- Direct Domain Service Exposure Without Registry Adapters (rejected)
  Rejected because: Directly binding internal domain services to protocol endpoints leaks domain models and couples business logic to external SDK interfaces.
  When valid: Valid only in rapid exploratory prototypes where domain models are trivial and stability across protocol upgrades is unneeded.

## Risks

- Breaking changes in upstream protocol SDK releases could disrupt tool registration and invocation contracts.
  Mitigation: Isolate protocol SDK calls within dedicated adapter modules and verify resolved versions against repository lock artifacts.
  Owner: Backend Engineering Team
- Inconsistent error propagation across tool endpoints could expose internal stack traces or leak sensitive domain state to external agents.
  Mitigation: Enforce strict schema validation and error sanitization within the tool adapter layer prior to protocol response dispatch.
  Owner: Security and Backend Engineering Teams

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement new agent tools as dedicated domain modules that register capabilities through the shared registry adapter rather than binding directly to protocol server instances.
- Ensure all tool input and output structures define formal schemas compatible with the underlying protocol SDK capabilities.

## Continuation Context


Verify commands:
- Discover the project test execution script from the repository manifest and execute the test suite for tool route handlers.
- Discover the project static analysis and linting scripts from the repository manifest and execute them across the server codebase.
- Discover the project build and type checking scripts from the repository manifest and verify compilation across all protocol integration endpoints.

Accept when:
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and type validation suites
- Verified by: Peer code review verifying adherence to protocol adapter isolation patterns
- Violation handling: Pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review.
- Violation handling: Build failures resulting from type mismatches or unverified dependency versions must block merge.
- Exception process: Exceptions require approval from backend architecture leads with an accompanying migration plan.