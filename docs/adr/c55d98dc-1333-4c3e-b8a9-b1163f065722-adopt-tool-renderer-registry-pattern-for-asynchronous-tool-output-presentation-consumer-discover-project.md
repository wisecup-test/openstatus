# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Consumer Discover Project

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system processes tool invocations from agent services and must present both draft states and final results to users through a dashboard interface
- Multiple tool types require distinct rendering logic, necessitating a registry pattern to map tool names to their corresponding presentation components
- Tool execution follows an asynchronous workflow where draft representations must be shown before final results arrive
- The rendering layer must remain decoupled from tool implementation details while supporting extensible addition of new tool types
- Integration between agent services and the dashboard requires a stable contract for tool output transformation

## Problem Statement

When agent services invoke tools asynchronously, the dashboard must present intermediate draft states and final results without tightly coupling rendering logic to each tool implementation, while supporting dynamic addition of new tool types and maintaining consistent presentation contracts across the integration boundary.

## Decision

1. MUST: The consumer MUST discover the project's dependency lock artifact and resolve the exact installed versions of all rendering framework dependencies before implementing or modifying tool renderers

## Policy Block

- MUST The consumer MUST discover the project's dependency lock artifact and resolve the exact installed versions of all rendering framework dependencies before implementing or modifying tool renderers

In scope:
- Tool output presentation components in dashboard interfaces
- Agent service tool invocation result handling
- Asynchronous tool execution state rendering
- Tool renderer registration and lookup logic

Out of scope:
- Tool execution logic within agent services
- Tool input validation and parameter processing
- Agent decision-making and tool selection
- Backend API endpoints for tool invocation

## Rationale

- The evidence shows explicit exports for ToolRenderer, ToolRendererRegistry, and three rendering functions (renderToolDraft, renderToolResult, summarizeToolOutput), indicating a formalized contract for tool presentation
- The detection of multiple tool renderer modules imported into a central index demonstrates a registry pattern that enables extensible addition of new tool types without modifying core rendering logic
- The presence of both draft and result rendering functions reflects the asynchronous nature of tool execution where intermediate states must be presented before final results arrive
- The pattern isolates presentation concerns from tool implementation, allowing agent services and dashboard components to evolve independently while maintaining a stable integration contract

## Consequences

Positive:
- Enables independent development of new tool renderers without modifying core registry or lookup logic
- Provides consistent presentation contracts across all tool types, improving maintainability
- Supports asynchronous tool execution workflows by separating draft and result rendering concerns
- Decouples agent service tool implementation from dashboard presentation logic

Negative:
- Introduces indirection that may complicate debugging when renderer lookup fails silently
- Requires developers to understand the registry pattern and interface contract when adding new tool types
- May lead to inconsistent user experience if tool renderers implement the interface differently
- Creates a central coordination point that could become a bottleneck for renderer updates

## Alternatives

- Implement tool-specific rendering logic directly in the dashboard component using conditional statements based on tool name (rejected)
  Rejected because: Creates tight coupling between dashboard components and tool implementations, making it difficult to add new tools without modifying core presentation logic and violating separation of concerns
  When valid: Only appropriate for prototypes with fewer than three tool types where extensibility is not a requirement
- Use a plugin architecture where tool renderers are dynamically loaded at runtime from separate packages (rejected)
  Rejected because: Adds significant complexity for dynamic loading and version management without clear benefit given the current evidence shows static imports and compile-time registration
  When valid: Appropriate when tool renderers are developed by third parties or must be deployed independently of the main application
- Embed rendering metadata in tool output payloads and use a generic renderer that interprets the metadata (deferred)
  Rejected because: null
  When valid: Could be reconsidered if tool types become highly dynamic or if rendering logic needs to be controlled by the agent service rather than the dashboard

## Risks

- Silent failures when tool renderers are not registered or lookup returns undefined, leading to blank or broken UI states
  Mitigation: Implement fallback rendering logic that displays raw tool output when no renderer is found, and add monitoring for renderer lookup failures
  Owner: engineering team
- Interface contract drift where new tool renderers implement the interface inconsistently, causing presentation bugs
  Mitigation: Define the renderer interface using strict typing and add integration tests that verify all registered renderers conform to the contract
  Owner: engineering team
- Registry becomes a coordination bottleneck as the number of tool types grows, requiring frequent updates to the central index
  Mitigation: Consider automated registry generation from module discovery or adopt a convention-based registration approach that reduces manual index maintenance
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
- Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization, using strict typing to enforce contract compliance across all implementations
- Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index to maintain clear separation and enable independent testing
- Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output, ensuring graceful degradation when new tool types are encountered before their renderers are implemented

## Continuation Context


Verify commands:
- Discover the project's test execution script in the dependency manifest or task runner configuration and execute the test suite covering tool renderer registration and lookup logic
- Discover the project's type checking configuration and execute the type checker to verify all tool renderer implementations conform to the standardized interface contract
- Discover the project's linting configuration and execute the linter to verify import organization and module structure for tool renderer components

Accept when:
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types

## Enforcement

- Verified by: Type checking in continuous integration pipeline verifies interface contract compliance
- Verified by: Unit tests validate registry lookup behavior and renderer function signatures
- Verified by: Code review process confirms new tool renderers follow the established pattern and are properly registered
- Violation handling: Type checking failures block merge until renderer implementations conform to the interface contract
- Violation handling: Missing renderer registrations trigger warnings in development environment and fallback to raw output rendering in production
- Violation handling: Code review identifies deviations from the pattern and requests changes before approval
- Exception process: Exceptions for non-standard renderer implementations require architectural review and documentation of the specific use case that cannot be satisfied by the standard interface
- Exception process: Temporary bypass of renderer registration is permitted for experimental tool types in development branches but must be resolved before production deployment
- Exception process: Fallback rendering for unregistered tools is the standard exception mechanism and does not require additional approval