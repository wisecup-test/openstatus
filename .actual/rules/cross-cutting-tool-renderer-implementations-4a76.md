# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Tool Renderer Implementations

These rules are ALWAYS ACTIVE for all agent tool integration implementations that require custom rendering logic for draft states and execution results in user interfaces.

### Rules

- **R-TOOL-RENDERER-001** MUST: All tool renderer implementations MUST conform to a standardized interface contract that defines both draft rendering and result rendering methods.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify renderer interface conformance across all implementations
type_checker=$(grep -E '(typescript|tsc|eslint)' package.json 2>/dev/null | head -1)
if [ -n "$type_checker" ]; then
  npm run type-check || tsc --noEmit
fi

# Locate the project's test suite and execute integration tests that verify
# renderer registry lookup and dispatch for all registered tool types
if [ -f "jest.config.js" ] || [ -f "vitest.config.ts" ]; then
  npm test -- --testPathPattern="renderer|registry" --testNamePattern="registry|lookup|dispatch"
fi

# Identify the project's linting configuration and run the linter to verify
# consistent renderer implementation patterns and interface adherence
if [ -f ".eslintrc" ] || [ -f ".eslintrc.json" ] || [ -f "eslint.config.js" ]; then
  npm run lint -- --rule "no-unused-vars: error" --rule "@typescript-eslint/explicit-function-return-types: error"
fi
```

**Accept when:**
- All tool renderer implementations successfully type-check against the standardized interface contract without type assertions or suppressions
- Registry lookup tests pass for all registered tool types and correctly return undefined for unregistered tools
- Integration tests demonstrate successful rendering of both draft and result phases for representative tool outputs, producing valid change row structures
- Linter verification confirms consistent renderer implementation patterns and interface adherence across all tool-specific renderers

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer implementations, or integration test failures block acceptance.
</enforcement>