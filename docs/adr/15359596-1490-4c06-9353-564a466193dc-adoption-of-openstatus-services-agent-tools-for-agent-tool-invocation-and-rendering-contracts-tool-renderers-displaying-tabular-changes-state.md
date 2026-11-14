---
version: 2
name: 15359596-1490-4c06-9353-564a466193dc-adoption-of-openstatus-services-agent-tools-for-agent-tool-invocation-and-rendering-contracts-tool-renderers-displaying-tabular-changes-state
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Renderers Displaying Tabular Changes State"
metadata:
  actual:
    adr_id: 15359596-1490-4c06-9353-564a466193dc
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Renderers Displaying Tabular Changes State

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- Conversational agent interactions generate structured operational payloads that require specialized user interface representations.
- Shared agent tool contracts are defined within @openstatus/services/agent-tools to standardize payload schemas across execution and presentation boundaries.
- The dashboard chat subsystem registers dedicated renderer components for known tools and falls back to structured raw output when an unknown tool identifier is encountered.

## Problem Statement

Rendering heterogeneous agent tool execution outputs in conversational interfaces without a centralized contract library leads to inconsistent payload parsing, fragile UI components, and unhandled rendering failures when new tools are introduced.

## Decision

1. SHOULD: Tool renderers displaying tabular changes or state diffs SHOULD reuse shared workspace table components to maintain presentation consistency across views.

## Policy Block

- SHOULD Tool renderers displaying tabular changes or state diffs SHOULD reuse shared workspace table components to maintain presentation consistency across views.

In scope:
- Interactive chat interfaces rendering agent tool invocations and operational outputs.
- Component registries mapping agent tool identifiers to dedicated presentation components.

Out of scope:
- Static administrative data tables rendering API payloads independently of the chat agent runtime.
- Backend tool execution handlers within service boundaries.

## Rationale

- Standardizing on @openstatus/services/agent-tools provides a single source of truth for tool schemas across both agent services and client rendering components.
- A dedicated renderer registry with fallback logging prevents conversational interface crashes when encountering newly deployed or unmapped tools while providing clear diagnostics for missing renderers.
- Modularizing individual tool renderers maintains separation of concerns and facilitates incremental additions of new agent capabilities.

## Consequences

Positive:
- Ensures type-safe and contract-aligned rendering of agent tool inputs and outputs across the user interface.
- Prevents interface crashes on unexpected tool payloads by providing structured raw fallback rendering with diagnostic warnings.
- Enables independent authoring and testing of specialized presentation components for each agent tool.

Negative:
- Requires maintaining continuous synchronization between tool definitions in the shared library and the frontend renderer registry.
- Introduces bundle overhead from registering multiple specialized rendering components within the conversational module.

## Alternatives

- Ad-hoc inline JSON rendering for all agent tool outputs (rejected)
  Rejected because: Lacks specialized visualization, resulting in poor user experience and unformatted technical output for end users.
  When valid: Acceptable during early prototyping when tool output schemas change rapidly.
- Independent schema redefinition in each frontend tool renderer component (rejected)
  Rejected because: Duplicating schemas leads to drift and silent runtime rendering bugs when backend tool signatures evolve.
  When valid: Valid when external tool contracts are untyped or cannot be shared across workspace boundaries.

## Risks

- New tools added to the shared package may lack corresponding UI renderers, displaying raw fallback output to users.
  Mitigation: Automated regression tests and fallback logging alert developers during development when unmapped tools are executed.
  Owner: engineering team
- Schema changes in the shared package may break consumer rendering assumptions if dependencies are updated asynchronously.
  Mitigation: Verify strict dependency lockfiles and execute monorepo type checks prior to deployment.
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
- Register each new tool renderer component in the central tool registry mapping corresponding to its contract in @openstatus/services/agent-tools.
- Ensure fallback logging captures unmapped tool names in development and diagnostic environments to accelerate renderer implementation.

## Continuation Context


Verify commands:
- Discover and run the workspace dependency verification command to ensure package resolution matches repository lock specifications.
- Discover and execute the project typecheck script to validate that all tool renderers conform to @openstatus/services/agent-tools contracts.
- Discover and run the test suite covering conversational tool rendering to confirm fallback handling for unmapped tool identifiers.

Accept when:
- All tool renderer components typecheck without errors against @openstatus/services/agent-tools contracts.
- The tool registry renders supported tools and triggers the expected fallback warning for unmapped tools.
- Workspace lockfile verification confirms all packages resolve to exact locked versions.

## Enforcement

- Verified by: Automated continuous integration type checking across workspace boundaries.
- Verified by: Pull request review verifying renderer registration for newly added agent tools.
- Verified by: Automated test suites validating fallback rendering behavior.
- Violation handling: Continuous integration failure for unmapped or misaligned tool type signatures.
- Violation handling: Code review rejection for unregistered or ad-hoc tool rendering implementations.
- Exception process: Architectural review approval required for bypassing the shared contract package or using unstructured raw display for production tools.