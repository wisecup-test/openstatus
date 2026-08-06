# Validate Search Parameters Through Centralized Cache Parser: Page Components Receiving

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters as promises from routing and must validate them before use in data fetching or routing decisions.

### Rules

- **R-PARAM-001** MUST: Page components receiving search parameters as promises MUST await the promise before passing to the validation parser.

### Verify

```bash
# Discover and run the project's test execution script covering page components with search parameter validation
# (Exact command depends on build tool found in dependency manifest)

# Discover and run the project's static analysis or type-checking script to verify type-safe search parameter access
# (Exact command depends on build tool found in dependency manifest)

# Discover and run the project's linting configuration to detect direct access to raw search parameters that bypass validation
# (Exact command depends on linter found in project configuration)
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No direct access to raw search parameters is detected that bypasses the centralized validation parser

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking and test suite execution are mandatory before accepting any page component changes involving search parameter handling.
</enforcement>