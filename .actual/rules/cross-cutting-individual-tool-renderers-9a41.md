# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Individual Tool Renderers

These rules are ALWAYS ACTIVE for all agent tool integration implementations that require custom rendering logic for draft states and execution results in user interfaces.

### Rules

- **R-RENDERER-001** MAY: Individual tool renderers MAY implement only the rendering phases relevant to their tool's lifecycle and interaction model.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify renderer interface conformance across all implementations
find . -name "tsconfig.json" -o -name "pyproject.toml" -o -name ".eslintrc*" | head -1

# Locate the project's test suite and execute integration tests that verify
# renderer registry lookup and dispatch for all registered tool types
find . -path "*/test*" -name "*renderer*" -o -path "*/spec*" -name "*renderer*" | head -5

# Identify the project's linting configuration and run the linter to verify
# consistent renderer implementation patterns and interface adherence
find . -name ".eslintrc*" -o -name "pylintrc" -o -name "ruff.toml" | head -1
```

**Accept when:**
- All tool renderer implementations successfully type-check against the standardized interface contract without type assertions or suppressions
- Registry lookup tests pass for all registered tool types and correctly return undefined for unregistered tools
- Integration tests demonstrate successful rendering of both draft and result phases for representative tool outputs, producing valid change row structures
- Renderer implementations validate input structure before type narrowing and provide meaningful error messages when inputs do not match expected schemas

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block merge until renderer implementations conform to the interface contract. Missing renderer implementations for new tools trigger automated review comments requesting registry registration.
</enforcement>