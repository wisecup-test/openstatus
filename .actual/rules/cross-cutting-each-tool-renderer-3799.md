# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Each Tool Renderer

These rules are ALWAYS ACTIVE for all tool renderer implementations, registry lookup logic, and dashboard presentation components that handle asynchronous tool output rendering.

### Rules

- **R-RENDERER-001** MUST: Each tool renderer MUST implement a standardized interface defining methods for draft rendering, result rendering, and output summarization.
- **R-RENDERER-002** MUST: Define the tool renderer interface with explicit method signatures using strict typing to enforce contract compliance across all implementations.
- **R-RENDERER-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index.
- **R-RENDERER-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output.
- **R-RENDERER-005** SHOULD: Add monitoring for renderer lookup failures to detect silent failures when tool renderers are not registered.
- **R-RENDERER-006** SHOULD: Add integration tests that verify all registered renderers conform to the standardized interface contract.

### Verify

```bash
# Discover and execute the project's test suite covering tool renderer registration and lookup logic
grep -r "test" package.json Makefile .github/workflows/ 2>/dev/null | head -5

# Discover and execute the project's type checking configuration
grep -r "typecheck\|tsc\|type-check" package.json tsconfig.json 2>/dev/null | head -5

# Discover and execute the project's linting configuration
grep -r "lint" package.json .eslintrc 2>/dev/null | head -5
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types
- Type checking verifies interface contract compliance across all renderer implementations
- Code review confirms new tool renderers follow the established pattern and are properly registered

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block merge until renderer implementations conform to the interface contract. Missing renderer registrations trigger warnings in development and fallback to raw output rendering in production.
</enforcement>