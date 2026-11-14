---
version: 2
name: 83531d10-1ca6-40bf-8520-ec7e72076716-modelcontextprotocol-sdk-server-mcp-js-adoption-for-model-context-protocol-server-protocol-tools-registered-through-adapter-mechanism
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tools Registered Through Adapter Mechanism"
metadata:
  actual:
    adr_id: 83531d10-1ca6-40bf-8520-ec7e72076716
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tools Registered Through Adapter Mechanism

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/mcp/**`

## Context

- The backend application requires exposing system capabilities and domain operations to agentic systems via the Model Context Protocol.
- Direct ad-hoc exposure of internal domain services without strict interface boundaries creates security, validation, and maintenance vulnerabilities.
- Adopting a standardized protocol server library alongside uniform tool adapters ensures consistent validation and safe dispatching across all exposed capabilities.

## Problem Statement

Exposing heterogeneous internal domain operations to autonomous agent clients requires a standardized protocol interface and rigorous boundary enforcement. Without a unified protocol server and consistent input and output validation, exposed operations risk runtime schema mismatches, fragmented registration lifecycles, and unverified data exchange across client boundaries.

## Decision

1. MUST: All protocol tools MUST be registered through an adapter mechanism that validates input schemas using inputSchema.parse before handler execution and validates output schemas using outputSchema.parse prior to returning results.

## Policy Block

- MUST All protocol tools MUST be registered through an adapter mechanism that validates input schemas using inputSchema.parse before handler execution and validates output schemas using outputSchema.parse prior to returning results.

In scope:
- All server endpoints and modules that implement Model Context Protocol interfaces or expose agent-facing tools.
- All tool definitions interfacing external protocol clients with backend services.

Out of scope:
- Internal service-to-service communications that do not operate across the Model Context Protocol.
- Standard HTTP REST or RPC routes that serve direct web application consumers.

## Rationale

- Standardizing on @modelcontextprotocol/sdk/server/mcp.js provides a compliant and canonical implementation of protocol server primitives across all agent-facing routes.
- Enforcing input and output validation via schemas at the registry adapter layer guarantees runtime type safety and shields domain services from malformed protocol payloads.
- Separating individual domain capabilities into discrete tool modules that register through a shared registry adapter promotes modularity and simplifies auditability.

## Consequences

Positive:
- Provides a uniform, standards-compliant interface for agentic systems interacting with backend domain services.
- Guarantees strict schema validation on all inputs and outputs crossing the protocol boundary.
- Decouples protocol transport and serialization details from internal business service implementations.
- Centralizes tool registration and discovery through a consistent adapter mechanism.

Negative:
- Introduces runtime serialization and schema parsing overhead on every tool invocation.
- Creates an architectural dependency on the protocol SDK server lifecycle and interface specifications.
- Requires maintaining synchronized schema definitions alongside internal service signature changes.

## Alternatives

- Custom In-House Protocol Server Implementation (rejected)
  Rejected because: Building a bespoke server for the protocol duplicates protocol maintenance overhead and increases the risk of specification non-compliance.
  When valid: Valid only when protocol requirements diverge completely from established specifications or runtime constraints prohibit third-party protocol dependencies.
- Direct Handler Registration Without Schema Adapter (rejected)
  Rejected because: Registering handlers directly without an adapter layer that parses input and output schemas exposes internal domain services to runtime typing anomalies and unvalidated inputs.
  When valid: Valid only in low-overhead internal micro-benchmarks where schema enforcement is handled at an outer network gateway.

## Risks

- Protocol specification drift or upstream SDK breaking changes causing integration failures across tool endpoints.
  Mitigation: Pin resolved versions using lock artifact grounding and automate contract verification tests across all registered tool schemas.
  Owner: engineering team
- Schema parsing performance bottlenecks during high-frequency agent tool execution.
  Mitigation: Benchmark and optimize schema validation routines, ensuring lean payloads and efficient serialization pathways.
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
- Ensure tool registry adapters map internal domain service results cleanly into protocol output envelopes, parsing both input payloads and return structures.
- Isolate individual tool sets into dedicated domain modules that register with the server instance during endpoint initialization.

## Continuation Context


Verify commands:
- Discover and run the project static type inspection script to verify type adherence across protocol modules.
- Discover and run the test execution suite to validate tool registration and input and output schema parsing behavior.
- Discover and run the repository linter to ensure consistency and adherence to structural conventions.

Accept when:
- All protocol server tool registrations pass schema validation checks with zero type errors.
- Automated test suites verify that invalid tool inputs are rejected before reaching domain services.
- Protocol tool execution returns properly structured and validated output envelopes.

## Enforcement

- Verified by: Continuous integration pipelines executing static analysis and test validation suites.
- Verified by: Pull request code reviews verifying adherence to the registry adapter pattern for new tool additions.
- Violation handling: Pull requests bypassing the registry adapter or omitting schema validation must be rejected.
- Violation handling: Static analysis or test suite failures in protocol route modules block merge to the primary branch.
- Exception process: Exceptions require documented architectural review and formal approval from the technical leadership team before implementation.