# Adopt Async Server Component Pattern with Parallel Data Prefetching: When Server Component

These rules are ALWAYS ACTIVE for server-side page components that accept searchParams props, route handlers that process user-supplied query parameters, and server components that coordinate multiple data dependencies before rendering.

### Rules

- **R-ASYNC-001** MUST: When a server component requires multiple independent data fetches, those fetches MUST be coordinated in parallel using Promise.all or equivalent concurrent coordination primitives.

### Verify

```bash
# Discover the project's type checking configuration and execute the type checker
# to verify all searchParams props are declared with Promise types
type_checker_command

# Locate the project's test suite and run integration tests that verify search
# parameter validation rejects invalid inputs before data fetching occurs
test_suite_command

# Identify the project's linting configuration and execute linters to detect
# unawaited Promise access in server component files
linter_command
```

**Accept when:**
- Type checking passes with no errors related to searchParams Promise resolution or unawaited async operations
- Integration tests demonstrate that invalid search parameters trigger validation failures before any query prefetching executes
- Code review confirms all server page components with searchParams props use Promise.all for independent data fetches

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block merge to protected branches. Failed integration tests prevent deployment progression. Code review findings require resolution before approval.
</enforcement>