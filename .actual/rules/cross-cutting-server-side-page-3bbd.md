# Validate Search Parameters Through Centralized Cache Parser: Server Side Page

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters from routing, async page functions that prefetch data based on query parameters, and route handlers where search parameters control application behavior or data access.

### Rules

- **R-PARAM-001** MUST: Server-side page components MUST parse search parameters through a centralized cache parser before using parameter values in application logic.

### Verify

```bash
# Discover and run the project's test execution script covering page components with search parameter validation
# (Exact command depends on project's build tool and test runner — consult dependency manifest)

# Discover and run the project's static analysis or type-checking script to verify type-safe search parameter access
# (Exact command depends on project's type checker — consult dependency manifest)

# Discover and run the project's linting configuration to detect direct access to raw search parameters
# (Exact command depends on project's linter — consult dependency manifest)
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No linting violations report direct access to raw search parameters that bypass validation

<enforcement>
Claude Code MUST NOT skip or defer verification. All page components receiving search parameters MUST be validated through the centralized cache parser before any application logic consumes them. Type safety and validation are mandatory prerequisites for merge.
</enforcement>