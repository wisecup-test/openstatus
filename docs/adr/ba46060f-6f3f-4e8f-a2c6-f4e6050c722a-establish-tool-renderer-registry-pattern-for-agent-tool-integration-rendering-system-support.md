# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Rendering System Support

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all agent tool integration implementations within the system.

## Context

- The system integrates agent tools from external service packages that require custom rendering logic for both draft states and execution results in user interfaces.
- Tool outputs must be transformed into structured change representations that can be displayed consistently across different tool types without coupling the rendering logic to individual tool implementations.
- A registry pattern enables dynamic lookup of tool-specific renderers based on tool names, allowing the system to support multiple tool types without hardcoding dependencies.
- The rendering layer must handle both pre-execution draft visualization and post-execution result presentation, requiring a dual-phase rendering contract.

## Problem Statement

Agent tool integrations require custom visualization logic that varies by tool type, but the core rendering infrastructure must remain decoupled from specific tool implementations to enable extensibility and maintain separation of concerns between tool execution and presentation layers.

## Decision

1. MUST: The rendering system MUST support dynamic dispatch to tool-specific renderers without requiring compile-time knowledge of all tool types.

## Policy Block

- MUST The rendering system MUST support dynamic dispatch to tool-specific renderers without requiring compile-time knowledge of all tool types.

In scope:
- All agent tool integrations that require user interface visualization
- Tool renderer implementations that transform tool inputs and outputs into display representations
- Registry lookup functions that dispatch to tool-specific rendering logic
- Components that consume tool rendering outputs for presentation

Out of scope:
- Tool execution logic and business logic within agent service packages
- Network communication protocols between agent services and the application
- Authentication and authorization for tool invocation
- Tool state persistence and caching mechanisms

Exceptions:
- EXC-001: A tool produces outputs that cannot be meaningfully represented as structured change rows and requires a completely custom visualization component

## Rationale

- The evidence shows a registry pattern with findRenderer lookup, renderDraft and renderResult methods, and transformation to ChangeRow structures, indicating a deliberate separation between tool execution and presentation concerns.
- The use of unknown input types with type narrowing (as never) demonstrates a design that accommodates heterogeneous tool data structures while maintaining type safety at the registry boundary.
- The optional return type (undefined) for renderDraft indicates the system supports tools with varying lifecycle phases, allowing flexibility in which rendering methods each tool implements.
- The detection of multiple tool imports and a centralized renderer index suggests this pattern is actively used across multiple tool types, validating its utility for extensible integration.

## Consequences

Positive:
- New agent tools can be integrated by implementing the renderer interface and registering in the central registry without modifying core rendering infrastructure.
- Tool-specific rendering logic remains isolated in dedicated modules, improving maintainability and reducing coupling between tool implementations.
- The registry pattern enables runtime extensibility, allowing tools to be added or removed without recompiling dependent components.
- Normalized change row outputs enable consistent table-based visualization across diverse tool types, improving user experience coherence.

Negative:
- The use of unknown types and runtime type narrowing shifts some type safety from compile-time to runtime, increasing the risk of type-related errors.
- The registry introduces an additional layer of indirection that may complicate debugging when renderer lookup or dispatch fails.
- Maintaining the renderer interface contract across multiple tool implementations creates coordination overhead when interface changes are required.
- The pattern assumes tool outputs can be normalized to change rows, which may not be appropriate for all tool types and could force awkward data transformations.

## Alternatives

- Implement tool-specific rendering logic directly in the consuming components using conditional logic based on tool names (rejected)
  Rejected because: This approach tightly couples rendering logic to consuming components, making it difficult to add new tools and violating separation of concerns. It also concentrates rendering knowledge in components that should focus on layout and composition.
  When valid: For prototypes or systems with a fixed, small number of tools where extensibility is not a requirement
- Use a plugin architecture where each tool package exports its own rendering components that are dynamically loaded at runtime (rejected)
  Rejected because: Dynamic component loading introduces complexity around module resolution, bundling, and type safety that exceeds the requirements of this integration pattern. The registry pattern provides sufficient extensibility with simpler implementation.
  When valid: For systems requiring true runtime plugin installation without redeployment, or when tool packages are developed and distributed independently by third parties
- Define a standardized tool output schema that all tools must conform to, eliminating the need for tool-specific renderers (rejected)
  Rejected because: Different tool types produce fundamentally different output structures that serve different purposes. Forcing a single schema would either be too restrictive for complex tools or too generic to be useful, and would constrain tool design.
  When valid: For systems where all tools perform similar operations with naturally homogeneous outputs, such as CRUD operations on uniform data models

## Risks

- Runtime type narrowing failures could cause rendering errors when tool output structures change without corresponding renderer updates
  Mitigation: Implement comprehensive runtime validation of tool outputs before type narrowing, with fallback rendering for unrecognized structures. Add integration tests that verify renderer compatibility with actual tool outputs.
  Owner: Engineering team
- The centralized registry could become a bottleneck for parallel development if multiple teams need to modify it simultaneously
  Mitigation: Structure the registry as a composition of individual renderer imports rather than a monolithic configuration object. Use clear ownership boundaries and code review processes for registry modifications.
  Owner: Engineering team
- Changes to the renderer interface contract could require coordinated updates across all tool renderer implementations, creating migration complexity
  Mitigation: Version the renderer interface and support multiple interface versions simultaneously during transition periods. Make interface methods optional where possible to allow incremental adoption of new capabilities.
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
- Define the renderer interface with explicit method signatures for renderDraft and renderResult, using generic type parameters to maintain type safety while allowing tool-specific input types. Consider making both methods optional to support tools that only implement one rendering phase.
- Structure the registry as a map from tool name strings to renderer objects, and provide a lookup function that returns undefined for unregistered tools. This allows consuming code to gracefully handle missing renderers without throwing exceptions.
- When implementing tool-specific renderers, validate input structure before type narrowing and provide meaningful error messages when inputs do not match expected schemas. Consider using runtime type validation libraries to formalize these checks.
- Document the expected structure of change row outputs to ensure consistency across renderer implementations. Include fields for operation type, affected resources, and before/after states to support comprehensive change visualization.

## Continuation Context


Verify commands:
- Discover the project's type checking configuration and execute the type checker to verify renderer interface conformance across all implementations
- Locate the project's test suite and execute integration tests that verify renderer registry lookup and dispatch for all registered tool types
- Identify the project's linting configuration and run the linter to verify consistent renderer implementation patterns and interface adherence

Accept when:
- All tool renderer implementations successfully type-check against the standardized interface contract without type assertions or suppressions
- Registry lookup tests pass for all registered tool types and correctly return undefined for unregistered tools
- Integration tests demonstrate successful rendering of both draft and result phases for representative tool outputs, producing valid change row structures

## Enforcement

- Verified by: Automated type checking in continuous integration pipeline verifies renderer interface conformance
- Verified by: Code review process checks that new tool integrations include corresponding renderer implementations and registry entries
- Verified by: Integration test suite validates renderer functionality for all registered tools
- Violation handling: Type checking failures block merge until renderer implementations conform to the interface contract
- Violation handling: Missing renderer implementations for new tools trigger automated review comments requesting registry registration
- Violation handling: Runtime rendering errors are logged with tool name and input structure to facilitate debugging and renderer updates
- Exception process: Request architecture review for tools requiring custom visualization approaches that cannot conform to the change row model
- Exception process: Document the exception rationale, alternative rendering contract, and tool-specific requirements in the architecture decision log
- Exception process: Implement custom rendering path with explicit type guards and error handling to prevent fallback to standard renderer logic