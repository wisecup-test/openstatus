# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Draft Rendering Functions

These rules are ALWAYS ACTIVE for all tool renderer implementations, registry lookup logic, and dashboard presentation components that handle asynchronous tool output in the detection pipeline.

### Rules

- **R-RENDERER-001** MUST: Draft rendering functions MUST accept tool input and return a structured change representation or undefined.
- **R-RENDERER-002** MUST: Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization using strict typing to enforce contract compliance across all implementations.
- **R-RENDERER-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index to maintain clear separation and enable independent testing.
- **R-RENDERER-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output, ensuring graceful degradation when new tool types are encountered before their renderers are implemented.
- **R-RENDERER-005** SHOULD: Implement fallback rendering logic that displays raw tool output when no renderer is found, and add monitoring for renderer lookup failures.
- **R-RENDERER-006** SHOULD: Define the renderer interface using strict typing and add integration tests that verify all registered renderers conform to the contract.

### Verify

```bash
# Discover and execute the project's test suite covering tool renderer registration and lookup logic
# (Exact command depends on project's test runner — check dependency manifest or task configuration)

# Discover and execute the project's type checking configuration to verify all tool renderer implementations conform to the standardized interface contract
# (Exact command depends on project's type checker — check tsconfig.json, pyproject.toml, or equivalent)

# Discover and execute the project's linting configuration to verify import organization and module structure for tool renderer components
# (Exact command depends on project's linter — check .eslintrc, .pylintrc, or equivalent)
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types
- Import organization and module structure for tool renderer components conform to the established pattern

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer registrations, or deviations from the established pattern MUST be resolved before code is committed. Fallback rendering for unregistered tools is the standard exception mechanism and does not require additional approval.
</enforcement>