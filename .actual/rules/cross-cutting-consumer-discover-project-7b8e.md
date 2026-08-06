# Validate Search Parameters Through Centralized Cache Parser: Consumer Discover Project

These rules are ALWAYS ACTIVE for all server-side page components that accept search parameters, route handlers that process URL query strings, and data fetching logic that depends on search parameter values.

### Rules

- **R-SEARCHPARAMS-001** MUST: Discover the project's dependency lock file and resolve the exact installed version of the search parameter validation library before implementing validation logic.
- **R-SEARCHPARAMS-002** MUST: All server-side page components that accept search parameters MUST invoke the centralized cache parser before accessing or consuming any parameter values.
- **R-SEARCHPARAMS-003** MUST: Await cache parser resolution before any conditional logic, data fetching operations, or downstream processing that depends on search parameter values.
- **R-SEARCHPARAMS-004** MUST: Wrap cache parser invocations in error handling to gracefully manage validation failures and prevent unhandled promise rejections.
- **R-SEARCHPARAMS-005** SHOULD: Structure optional or default-valued parameters in the cache parser schema definition rather than handling defaults in component logic after parsing.
- **R-SEARCHPARAMS-006** SHOULD: Coordinate asynchronous validation with Promise.all to ensure parsing completes before downstream operations, maintaining temporal ordering of security controls.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting or type-checking
# to verify search parameter usage patterns
find . -name '.eslintrc*' -o -name 'tsconfig.json' -o -name 'biome.json' | head -1

# Locate and execute the test suite to verify cache parser integration
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1

# Search for page component definitions that accept search parameters
grep -r 'searchParams' --include='*.tsx' --include='*.ts' --include='*.jsx' --include='*.js' | grep -E '(page|component)' | head -20

# Verify each page component invokes the cache parser before parameter consumption
grep -r 'searchParamsCache\.parse\|cache.*parse' --include='*.tsx' --include='*.ts' | wc -l
```

**Accept when:**
- The exact installed version of the search parameter validation library is identified from the project's lock file
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters
- Type checking enforcement ensures search parameters are accessed only after cache parser resolution
- Error handling is present around all cache parser invocations

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope. Violations must be identified and remediated before proceeding.
</enforcement>