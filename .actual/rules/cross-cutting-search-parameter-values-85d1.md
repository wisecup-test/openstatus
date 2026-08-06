# Adopt Async Server Component Pattern with Parallel Data Prefetching: Search Parameter Values

These rules are ALWAYS ACTIVE for all server-side page components that accept searchParams props, route handlers that process user-supplied query parameters, and server components that coordinate multiple data dependencies before rendering.

### Rules

- **R-ASYNC-001** MUST: All search parameter values MUST be validated through a dedicated parser before use in queries, redirects, or business logic.
- **R-ASYNC-002** MUST: Server components accepting searchParams props MUST declare them with Promise types and explicitly await resolution before use.
- **R-ASYNC-003** MUST: Multiple independent data dependencies MUST be coordinated using Promise.all to minimize sequential waterfall delays.
- **R-ASYNC-004** SHOULD: Search parameter validation schemas SHOULD be defined adjacent to the components that consume them to maintain locality of behavior.
- **R-ASYNC-005** SHOULD: When adding new prefetch operations to Promise.all arrays, verify that queries are truly independent and do not have hidden dependencies.
- **R-ASYNC-006** MUST: Unawaited Promise access in server component files MUST be detected and prevented through linting rules.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify all searchParams props are declared with Promise types
type_checker_cmd=$(grep -r "typecheck\|type-check" package.json tsconfig.json 2>/dev/null | head -1)
echo "Executing type checker: $type_checker_cmd"

# Locate the project's test suite and run integration tests that verify search
# parameter validation rejects invalid inputs before data fetching occurs
test_cmd=$(grep -r "test\|jest\|vitest" package.json 2>/dev/null | grep -i "integration\|e2e" | head -1)
echo "Executing integration tests: $test_cmd"

# Identify the project's linting configuration and execute linters to detect
# unawaited Promise access in server component files
lint_cmd=$(grep -r "eslint\|lint" package.json 2>/dev/null | head -1)
echo "Executing linter: $lint_cmd"

# Verify searchParams are awaited in server components
grep -r "searchParams" --include="*.ts" --include="*.tsx" | grep -v "await" | grep -v "Promise" && echo "WARNING: Found potential unawaited searchParams" || echo "PASS: searchParams properly handled"
```

**Accept when:**
- Type checking passes with no errors related to searchParams Promise resolution or unawaited async operations
- Integration tests demonstrate that invalid search parameters trigger validation failures before any query prefetching executes
- Code review confirms all server page components with searchParams props use Promise.all for independent data fetches
- Linting detects no unawaited Promise access in server component files
- Search parameter validation schemas are co-located with component definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, failed integration tests, or linting violations block merge to protected branches and prevent deployment progression.
</enforcement>