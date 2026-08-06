# Adopt Tool Renderer Registry Pattern for Asynchronous Tool Output Presentation: Tool Renderers Organized

These rules are ALWAYS ACTIVE for all tool renderer implementations, registry index files, and dashboard presentation components that handle asynchronous tool output rendering.

### Rules

- **R-TOOL-RENDERER-001** SHOULD: Tool renderers SHOULD be organized as separate modules and imported into the registry index to maintain separation of concerns.
- **R-TOOL-RENDERER-002** MUST: Define the tool renderer interface with explicit method signatures for draft rendering, result rendering, and output summarization, using strict typing to enforce contract compliance across all implementations.
- **R-TOOL-RENDERER-003** MUST: Organize tool renderer modules in a dedicated directory structure where each tool type has its own module, then import and register them in the central index to maintain clear separation and enable independent testing.
- **R-TOOL-RENDERER-004** MUST: Implement the registry lookup function to return undefined for unregistered tools and provide a fallback renderer that displays raw output, ensuring graceful degradation when new tool types are encountered before their renderers are implemented.

### Verify

```bash
# Discover and execute the project's test suite covering tool renderer registration and lookup logic
test_script=$(grep -E '"test"|test:' package.json | head -1 | cut -d'"' -f4)
npm run "$test_script" -- --testPathPattern="renderer|registry"

# Discover and execute type checking to verify all tool renderer implementations conform to the interface
type_check_script=$(grep -E '"type-check"|"typecheck"|"tsc"' package.json | head -1 | cut -d'"' -f4)
npm run "$type_check_script"

# Discover and execute linting to verify import organization and module structure
lint_script=$(grep -E '"lint"' package.json | head -1 | cut -d'"' -f4)
npm run "$lint_script" -- --fix
```

**Accept when:**
- All registered tool renderers successfully implement the standardized interface and pass type checking without errors
- Registry lookup function correctly returns renderer implementations for registered tools and undefined for unregistered tools
- Test suite demonstrates that draft rendering, result rendering, and output summarization functions execute without runtime errors for all registered tool types
- Linting confirms import organization follows the established module structure pattern

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer registrations, or deviations from the established pattern MUST be resolved before code is committed.
</enforcement>