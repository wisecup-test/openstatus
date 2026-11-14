---
version: 2
name: eb18331f-6eef-47ab-b0b9-f72a405f5b5d-adoption-of-ai-sdk-openai-compatible-for-model-provider-resolution-system-route-openai-compatible-language-model
description: "Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: System Route Openai Compatible Language Model"
metadata:
  actual:
    adr_id: eb18331f-6eef-47ab-b0b9-f72a405f5b5d
    status: accepted
    candidate_group_id: c5f02e5f-768e-49c4-9e06-66844f1be5cb
    evidence:
      source: deep
  scope:
    root: packages/ai
    paths:
      - src/**
---

# Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: System Route Openai Compatible Language Model

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ai`

Applies to:
- `src/**`

## Context

- Systems integrating generative artificial intelligence require flexible model resolution to support alternative inference endpoints without coupling domain logic to vendor-specific protocols.
- The application isolates model resolution within a dedicated AI package that interfaces with database schema definitions to configure backend endpoints.
- The project adopts @ai-sdk/openai-compatible to serve as the unified adapter for all language model endpoints adhering to the OpenAI-compatible specification.

## Problem Statement

Coupling model consumption directly to proprietary vendor SDKs creates architectural friction when routing requests across heterogeneous OpenAI-compatible model hosts. Without a unified provider adapter, the system must maintain divergent request formats and authentication flows, increasing maintenance overhead and leaking endpoint transport details into core workflows.

## Decision

1. MUST: The system MUST route all OpenAI-compatible language model resolution through @ai-sdk/openai-compatible provider adapters rather than direct HTTP client calls or proprietary vendor client libraries.

## Policy Block

- MUST The system MUST route all OpenAI-compatible language model resolution through @ai-sdk/openai-compatible provider adapters rather than direct HTTP client calls or proprietary vendor client libraries.

In scope:
- Model resolution, provider instantiation, and language model routing within the dedicated AI subsystem.
- Integration of any inference endpoint implementing the OpenAI-compatible HTTP API specification.

Out of scope:
- Non-generative machine learning services and rule-based evaluation engines.
- Direct database schema migrations and data access logic outside model resolution configuration.

## Rationale

- Adopting @ai-sdk/openai-compatible standardizes endpoint communication against a single protocol adapter, eliminating redundant client libraries for compatible hosting platforms.
- Centralizing model instantiation within the dedicated AI module preserves loose coupling between application workflows and model backend infrastructure.
- Interfacing with workspace schema definitions allows model connection parameters to be persisted and managed dynamically across environments.

## Consequences

Positive:
- Enables seamless switching and routing between OpenAI-compatible model backends without altering consumer invocation code.
- Reduces dependency sprawl by utilizing a single standard adapter for multiple inference providers.
- Isolates network transport and authentication mapping into a dedicated model resolution boundary.

Negative:
- Restricts backend model capabilities to features supported by the OpenAI-compatible specification and adapter interface.
- Requires ongoing verification that self-hosted or third-party backends strictly conform to expected response streaming and error payload schemas.

## Alternatives

- Direct Custom HTTP Fetch Client (rejected)
  Rejected because: Implementing raw HTTP client wrappers increases boilerplate, requires manual stream handling, and fails to integrate with unified AI stream interfaces.
  When valid: Valid only in constrained runtime environments where third-party package dependencies cannot be introduced.
- Proprietary Individual Vendor SDKs (rejected)
  Rejected because: Binding to proprietary client packages fragments model resolution logic and couples the application to vendor-specific SDK lifecycles.
  When valid: Valid when utilizing proprietary capabilities not exposed via OpenAI-compatible endpoints.

## Risks

- Upstream breaking changes in provider adapter factory signatures across dependency updates.
  Mitigation: Verify resolved dependency lock artifacts and enforce comprehensive automated test suites covering provider resolution.
  Owner: AI Platform Team
- Subtle API incompatibilities or non-standard token streaming behaviors in self-hosted model backends.
  Mitigation: Implement validation checks and normalization wrappers at the model resolution layer.
  Owner: AI Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure model resolution factories to accept endpoint URLs and credential references dynamically from resolved database configuration records.
- Ensure all exported model instances conform to standard language model interface contracts expected by downstream AI workflows.

## Continuation Context


Verify commands:
- Discover the repository package manifest and execute the configured test suite to verify model resolution behavior across configured provider endpoints.
- Discover and run the project static analysis and type checking scripts to verify contract conformity for all provider factory invocations.

Accept when:
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

## Enforcement

- Verified by: Continuous integration pipeline running automated tests and static type checking across the AI module.
- Verified by: Mandatory architectural peer review on pull requests modifying model resolution or provider configuration logic.
- Violation handling: Pull requests containing direct imports of @ai-sdk/openai-compatible outside the model resolution layer will be blocked from merging.
- Violation handling: Violations detected in static analysis must be refactored to consume models exclusively via the authorized resolution interface.
- Exception process: Submit an architectural review request with justification when a backend requires proprietary extensions unsupported by the standard provider adapter.
- Exception process: Exceptions require documented approval from the technical lead and must be isolated within dedicated adapter wrappers.