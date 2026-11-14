---
version: 2
name: 42acf5e0-e40a-4744-8348-1ec5a3892867-adoption-of-ai-library-for-server-agent-orchestration-and-evaluation-agent-definitions-resolve-model-identifiers-through
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Definitions Resolve Model Identifiers Through"
metadata:
  actual:
    adr_id: 42acf5e0-e40a-4744-8348-1ec5a3892867
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Definitions Resolve Model Identifiers Through

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Server-side services require automated agent orchestration, structured tool execution, and quantitative model evaluations across messaging integrations and protocol endpoints.
- Prior to adopting a unified foundation, model invocations and tool schemas risked fragmented implementations across different server route handlers.
- The codebase establishes a unified dependency on the ai library across agent definitions, tool registry runners, and evaluation harnesses to standardize model interaction.

## Problem Statement

Server-side conversational agents and evaluation harnesses require a standardized, robust interface for model execution, tool calling, and prompt orchestration. Without a unified library adoption, separate route implementations risk diverging in tool parameter validation, execution tracking, and model configuration handling.

## Decision

1. MUST: Agent definitions MUST resolve model identifiers through runtime environment configuration rather than hardcoding static model names.

## Policy Block

- MUST Agent definitions MUST resolve model identifiers through runtime environment configuration rather than hardcoding static model names.

In scope:
- Server-side routes and services implementing autonomous agents, tool execution registries, or model evaluation harnesses.
- Modules integrating conversational agent workflows with external communication protocols or client adapters.

Out of scope:
- Static server routes, traditional CRUD endpoints, and database migrations that do not interact with language models.
- Client-side user interface rendering logic not participating in server-side agent execution.

## Rationale

- Adopting the ai library provides a consistent abstraction for coordinating model invocations and tool-calling execution loops across server routes.
- Coupling the ai library with strict input schema validation guarantees that agent tool executions are defensively parsed before invocation.
- Standardizing on a single library across both runtime agent execution and evaluation suites enables consistent testing of tool choices and reasoning behavior.

## Consequences

Positive:
- Unifies agent orchestration and model interaction patterns across server route handlers.
- Improves runtime safety by pairing agent tool invocation pipelines with strict input validation schemas.
- Facilitates standardized evaluation and telemetry collection for agent tool selection and reasoning traces.

Negative:
- Introduces an external dependency coupling for core server agent routing and tool execution.
- Requires developers to adhere to the adopted library calling conventions and upstream interface updates.

## Alternatives

- Bespoke Direct Model API Clients (rejected)
  Rejected because: Maintains high maintenance overhead, inconsistent tool calling schemas, and duplicate execution plumbing across server routes.
  When valid: When a minimal single-endpoint integration does not require tool execution, agent loops, or evaluation harnesses.
- Protocol-Specific Agent Frameworks (rejected)
  Rejected because: Ties agent execution logic to specific transport protocols rather than a composable library architecture usable across both evaluations and integrations.
  When valid: When the application is exclusively bound to a single communication platform ecosystem.

## Risks

- Breaking interface updates across upstream releases of the adopted library could impact agent tool calling runtime stability.
  Mitigation: Strict lock-version grounding and automated typecheck and test verification in continuous integration before library upgrades.
  Owner: engineering team
- Unvalidated tool inputs could cause runtime execution failures during agent loops.
  Mitigation: Enforce schema parsing on all tool input payloads prior to execution.
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
- Define all agent tools with explicit schema definitions and validate input payloads defensively before dispatching actions.
- Externalize target model identifiers to environment configurations so model endpoints can be updated across environments without modifying agent orchestrator code.

## Continuation Context


Verify commands:
- Discover the project script runner and run the test suite covering agent and evaluation routes.
- Discover and run the project typecheck script across server packages to verify compliance with the adopted library interfaces.
- Discover and run the repository linter to ensure agent tool definitions satisfy schema validation constraints.

Accept when:
- The project test suite passes with all agent orchestration and evaluation tests succeeding.
- The project static analysis and type verification checks complete with zero errors across agent route implementations.
- All tool definitions invoked by the agent runner conform to parsed input validation schemas without unvalidated parameter passthrough.

## Enforcement

- Verified by: Continuous integration test pipelines executing agent evaluation and tool execution suites.
- Verified by: Automated static analysis and type checking verifying library interface compliance.
- Verified by: Peer code review for all pull requests modifying server agent routes or tool registries.
- Violation handling: Pull requests introducing direct bespoke model calls or bypassing tool schema validation are blocked by continuous integration checks.
- Violation handling: Code review requires refactoring unauthorized model client implementations to use the adopted library.
- Exception process: Exceptions require documented architectural review and approval by the engineering team when third-party provider requirements cannot be met by the adopted library.