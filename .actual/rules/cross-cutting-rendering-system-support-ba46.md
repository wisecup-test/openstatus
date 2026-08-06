# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Rendering System Support

These rules are ALWAYS ACTIVE for all agent tool integration implementations that require custom rendering logic for draft states and execution results in user interfaces.

### Rules

- **R-RENDER-001** MUST: The rendering system MUST support dynamic dispatch to tool-specific renderers without requiring compile-time knowledge of all tool types.
- **R-RENDER-002** MUST: Define the renderer interface with explicit method signatures for `renderDraft` and `renderResult`, using generic type parameters to maintain type safety while allowing tool-specific input types.
- **R-RENDER-003** MUST: Make both `renderDraft` and `renderResult` methods optional to support tools that only implement one rendering phase.
- **R-RENDER-004** MUST: Structure the registry as a map from tool name strings to renderer objects, and provide a lookup function that returns `undefined` for unregistered tools.
- **R-RENDER-005** MUST: Validate input structure before type narrowing in tool-specific renderers and provide meaningful error messages when inputs do not match expected schemas.
- **R-RENDER-006** SHOULD: Use runtime type validation libraries to formalize input structure checks.
- **R-RENDER-007** MUST: Document the expected structure of change row outputs to ensure consistency across renderer implementations, including fields for operation type, affected resources, and before/after states.
- **R-RENDER-008** MUST: Implement comprehensive runtime validation of tool outputs before type narrowing, with fallback rendering for unrecognized structures.
- **R-RENDER-009** MUST: Add integration tests that verify renderer compatibility with actual tool outputs.
- **R-RENDER-010** MUST: Structure the registry as a composition of individual renderer imports rather than a monolithic configuration object.
- **R-RENDER-011** MUST: For tools requiring custom visualization approaches that cannot conform to the change row model, request architecture review and document the exception rationale in the architecture decision log.
- **R-RENDER-012** MUST: Implement custom rendering paths with explicit type guards and error handling to prevent fallback to standard renderer logic for exceptions.

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
  npm test -- --testPathPattern="renderer|registry" --coverage
fi

# Identify the project's linting configuration and run the linter to verify
# consistent renderer implementation patterns and interface adherence
if [ -f ".eslintrc" ] || [ -f ".eslintrc.json" ] || [ -f "eslint.config.js" ]; then
  npm run lint -- --fix
fi
```

**Accept when:**
- All tool renderer implementations successfully type-check against the standardized interface contract without type assertions or suppressions
- Registry lookup tests pass for all registered tool types and correctly return `undefined` for unregistered tools
- Integration tests demonstrate successful rendering of both draft and result phases for representative tool outputs, producing valid change row structures
- Linting passes with no violations of renderer implementation patterns
- Runtime validation of tool outputs prevents type narrowing failures and produces meaningful error messages for malformed inputs

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer implementations, or integration test failures MUST block merge until resolved. Runtime rendering errors MUST be logged with tool name and input structure to facilitate debugging and renderer updates.
</enforcement>