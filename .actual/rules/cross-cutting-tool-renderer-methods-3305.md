# Establish Tool Renderer Registry Pattern for Agent Tool Integration: Tool Renderer Methods

These rules are ALWAYS ACTIVE for all agent tool integration implementations that require custom rendering logic for draft states and execution results in user interfaces.

### Rules

- **R-TOOL-RENDERER-001** MUST: Tool renderer methods MUST accept unknown input types and perform runtime type narrowing to handle tool-specific data structures.

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
- Runtime type narrowing is implemented with explicit validation before type coercion, with meaningful error messages for schema mismatches

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block merge until renderer implementations conform to the interface contract. Missing renderer implementations for new tools trigger automated review comments requesting registry registration.
</enforcement>