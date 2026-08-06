# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Tool Rendering Logic

These rules are ALWAYS ACTIVE for all tool renderer implementations, registry lookup logic, and dashboard presentation components that handle asynchronous tool output.

### Rules

- **R-TOOL-RENDER-001** MUST: Tool rendering logic MUST be registered in a centralized registry that maps tool names to renderer implementations.
- **R-TOOL-RENDER-002** MUST: Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization using strict typing to enforce contract compliance.
- **R-TOOL-RENDER-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index.
- **R-TOOL-RENDER-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output.
- **R-TOOL-RENDER-005** SHOULD: Add monitoring for renderer lookup failures to detect silent failures when tool renderers are not registered.
- **R-TOOL-RENDER-006** SHOULD: Add integration tests that verify all registered renderers conform to the standardized interface contract.

### Verify

```bash
# Discover and execute the project's test suite covering tool renderer registration and lookup logic
# (Exact command depends on project's test runner — check dependency manifest or task configuration)

# Discover and execute the project's type checker to verify all tool renderer implementations conform to the interface
# (Exact command depends on project's type checking configuration)

# Discover and execute the project's linter to verify import organization and module structure
# (Exact command depends on project's linting configuration)
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types
- Type checking in continuous integration pipeline verifies interface contract compliance
- Code review confirms new tool renderers follow the established pattern and are properly registered

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block merge until renderer implementations conform to the interface contract. Missing renderer registrations trigger warnings in development and fallback to raw output rendering in production. Code review identifies deviations from the pattern before approval.
</enforcement>