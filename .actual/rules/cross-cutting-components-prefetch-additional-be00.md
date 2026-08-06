# Adopt Async Server Component Pattern with Parallel Data Prefetching: Components Prefetch Additional

These rules are ALWAYS ACTIVE for server-side page components that accept searchParams props, route handlers that process user-supplied query parameters, and server components that coordinate multiple data dependencies before rendering.

### Rules

- **R-ASYNC-001** MAY: Components MAY prefetch additional anticipated data beyond immediate requirements if cache invalidation strategies support speculative loading.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify all searchParams props are declared with Promise types
type_checker_cmd=$(grep -r "typecheck\|type-check" package.json 2>/dev/null | head -1 || echo "tsc")

# Locate the project's test suite and run integration tests that verify search parameter
# validation rejects invalid inputs before data fetching occurs
test_cmd=$(grep -r "test\|jest\|vitest" package.json 2>/dev/null | head -1 || echo "npm test")

# Identify the project's linting configuration and execute linters to detect
# unawaited Promise access in server component files
lint_cmd=$(grep -r "lint\|eslint" package.json 2>/dev/null | head -1 || echo "npm run lint")
```

**Accept when:**
- Type checking passes with no errors related to searchParams Promise resolution or unawaited async operations
- Integration tests demonstrate that invalid search parameters trigger validation failures before any query prefetching executes
- Code review confirms all server page components with searchParams props use Promise.all for independent data fetches

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures, failed integration tests, or code review findings indicating non-compliance with async patterns block acceptance.
</enforcement>