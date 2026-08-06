# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Registry Provide Optional

These rules are ALWAYS ACTIVE for all agent tool integration implementations that require custom rendering logic for draft states and execution results in user interfaces.

### Rules

- **R-REGISTRY-001** SHOULD: The registry SHOULD provide optional rendering methods that return undefined when a tool does not support a particular rendering phase.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify renderer interface conformance across all implementations
type_checker=$(grep -E '(typescript|tsc|eslint)' package.json 2>/dev/null | head -1)
if [ -n "$type_checker" ]; then
  npm run type-check || npx tsc --noEmit
fi

# Locate the project's test suite and execute integration tests that verify
# renderer registry lookup and dispatch for all registered tool types
if [ -f "jest.config.js" ] || [ -f "vitest.config.ts" ]; then
  npm test -- --testPathPattern="(renderer|registry)" --coverage
fi

# Identify the project's linting configuration and run the linter to verify
# consistent renderer implementation patterns and interface adherence
if [ -f ".eslintrc" ] || [ -f ".eslintrc.json" ]; then
  npx eslint "src/**/*renderer*.ts" --max-warnings 0
fi
```

**Accept when:**
- All tool renderer implementations successfully type-check against the standardized interface contract without type assertions or suppressions
- Registry lookup tests pass for all registered tool types and correctly return undefined for unregistered tools
- Integration tests demonstrate successful rendering of both draft and result phases for representative tool outputs, producing valid change row structures
- Linting passes with no violations in renderer implementation patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, missing renderer implementations, or test failures block acceptance.
</enforcement>