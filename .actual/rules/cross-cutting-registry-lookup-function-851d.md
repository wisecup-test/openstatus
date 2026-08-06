# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Registry Lookup Function

These rules are ALWAYS ACTIVE for all tool renderer registration, lookup logic, and dashboard presentation components that handle asynchronous tool output rendering.

### Rules

- **R-REGISTRY-001** MUST: The registry lookup function MUST accept a tool name string and return the corresponding renderer or undefined if no renderer is registered.
- **R-REGISTRY-002** MUST: Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization, using strict typing to enforce contract compliance across all implementations.
- **R-REGISTRY-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index to maintain clear separation and enable independent testing.
- **R-REGISTRY-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output, ensuring graceful degradation when new tool types are encountered before their renderers are implemented.
- **R-REGISTRY-005** SHOULD: Add monitoring for renderer lookup failures to detect silent failures when tool renderers are not registered.
- **R-REGISTRY-006** SHOULD: Consider automated registry generation from module discovery or adopt a convention-based registration approach that reduces manual index maintenance as the number of tool types grows.

### Verify

```bash
# Discover and execute the project's test suite covering tool renderer registration and lookup logic
# (Exact command depends on project's build tool and test runner configuration)

# Discover and execute the project's type checking configuration to verify all tool renderer implementations conform to the standardized interface contract
# (Exact command depends on project's type checker configuration)

# Discover and execute the project's linting configuration to verify import organization and module structure for tool renderer components
# (Exact command depends on project's linter configuration)
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types
- Import organization and module structure for tool renderer components conform to linting standards

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer registrations, or deviations from the established pattern MUST be resolved before code is considered complete.
</enforcement>