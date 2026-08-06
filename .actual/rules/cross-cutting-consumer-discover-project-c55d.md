# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Consumer Discover Project

These rules are ALWAYS ACTIVE for all tool renderer implementations, registry lookup logic, and dashboard presentation components that handle asynchronous tool output in the consumer discover project.

### Rules

- **R-TOOLRENDERER-001** MUST: The consumer MUST discover the project's dependency lock artifact and resolve the exact installed versions of all rendering framework dependencies before implementing or modifying tool renderers.
- **R-TOOLRENDERER-002** MUST: Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization, using strict typing to enforce contract compliance across all implementations.
- **R-TOOLRENDERER-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index to maintain clear separation and enable independent testing.
- **R-TOOLRENDERER-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output, ensuring graceful degradation when new tool types are encountered before their renderers are implemented.
- **R-TOOLRENDERER-005** SHOULD: All registered tool renderers successfully implement the standardized interface and pass type checking without errors.
- **R-TOOLRENDERER-006** SHOULD: Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools.
- **R-TOOLRENDERER-007** SHOULD: Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest or task runner configuration and execute the test suite covering tool renderer registration and lookup logic
# (Consumer must identify test runner from project configuration)

# Discover the project's type checking configuration and execute the type checker to verify all tool renderer implementations conform to the standardized interface contract
# (Consumer must identify type checker from project configuration)

# Discover the project's linting configuration and execute the linter to verify import organization and module structure for tool renderer components
# (Consumer must identify linter from project configuration)
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer registrations, or deviations from the established pattern block implementation until resolved.
</enforcement>