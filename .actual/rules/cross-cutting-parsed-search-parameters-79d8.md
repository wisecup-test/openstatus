# Validate Search Parameters Through Centralized Cache Parser: Parsed Search Parameters

These rules are ALWAYS ACTIVE for all server-side page components that accept search parameters, route handlers that process URL query strings, and data fetching logic that depends on search parameter values.

### Rules

- **R-SEARCH-001** SHOULD: Parsed search parameters SHOULD be destructured to extract only the specific values required by the page component.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting or type-checking commands to verify search parameter usage patterns
# Locate test files corresponding to page components and execute the test suite to verify cache parser integration
# Search the codebase for page component definitions that accept search parameters and verify each invokes the cache parser before parameter consumption
```

**Accept when:**
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations are caught by automated static analysis in CI, code review checklists, and type checking enforcement. Runtime monitoring alerts on validation failures or unhandled promise rejections from cache parser invocations.
</enforcement>