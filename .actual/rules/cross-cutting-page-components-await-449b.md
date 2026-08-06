# Validate Search Parameters Through Centralized Cache Parser: Page Components Await

These rules are ALWAYS ACTIVE for all server-side page components that accept search parameters from the framework router, route handlers that process URL query strings, and data fetching logic that depends on search parameter values.

### Rules

- **R-PARAMS-001** MUST: Page components MUST await the resolution of the search parameters Promise before accessing parameter values.
- **R-PARAMS-002** MUST: All search parameter validation MUST occur through the centralized searchParamsCache parser before any parameter consumption by application logic or data access layers.
- **R-PARAMS-003** MUST: Page components MUST invoke cache parser resolution before any conditional logic or data fetching operations that depend on search parameters.
- **R-PARAMS-004** SHOULD: Optional or default-valued parameters SHOULD be declared explicitly in the cache parser schema rather than handled in component logic after parsing.
- **R-PARAMS-005** MUST: Validation errors from the cache parser MUST be handled gracefully with error boundaries and fallback behavior to prevent unhandled promise rejections.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting/type-checking
# to verify search parameter usage patterns
find . -name '.eslintrc*' -o -name 'tsconfig.json' -o -name 'biome.json' | head -1

# Locate and execute the test suite for page components
find . -path '*/test*' -name '*page*.test.*' -o -name '*component*.test.*' | head -5

# Search for page component definitions accepting search parameters
grep -r 'searchParams' --include='*.tsx' --include='*.ts' --include='*.jsx' --include='*.js' | grep -E '(page|component)' | head -10

# Verify cache parser invocations in page components
grep -r 'searchParamsCache\.parse\|await.*searchParams' --include='*.tsx' --include='*.ts' | head -10

# Check for unvalidated search parameter access patterns
grep -r 'searchParams\[' --include='*.tsx' --include='*.ts' | grep -v 'await' | head -5
```

**Accept when:**
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters
- No page components access search parameters without prior cache parser resolution
- Validation errors are wrapped in error boundaries with documented fallback behavior

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code touching server-side page components with search parameters. Violations must be caught during code review and CI execution before merge.
</enforcement>