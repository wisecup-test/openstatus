# Validate Search Parameters Through Centralized Cache Parser: Parsed Search Parameters

These rules are ALWAYS ACTIVE for all server-side page components that accept searchParams props, async page functions that perform data prefetching based on URL parameters, and components that implement conditional routing or redirects based on query parameters.

### Rules

- **R-SEARCH-001** SHOULD: Parsed search parameters SHOULD be destructured immediately after parsing to make dependencies explicit.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
echo "Running type-checking workflow..."

# Locate the project's test suite and execute tests covering search parameter validation
echo "Running search parameter validation tests..."

# Identify the project's linting configuration and run the linter to detect direct
# searchParams access without cache parser invocation
echo "Running linter to detect unvalidated searchParams access..."
```

**Accept when:**
- All server-side page components that accept searchParams props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to searchParams properties occurs without prior cache parser invocation
- Search parameter schemas are defined in dedicated files co-located with page components
- Cache parser invocation occurs immediately after the async searchParams prop is received, before any conditional logic or data fetching operations

<enforcement>
Claude Code MUST NOT skip or defer verification. Static type checking in the continuous integration pipeline, code review process, and automated linting rules are mandatory enforcement mechanisms. Violations block deployment and require revision before merge approval.
</enforcement>