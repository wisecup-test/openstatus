# Adopt Cache Layer for Event Data in Handler Functions: Cache Layer Provide

These rules are ALWAYS ACTIVE for all handler functions in the checker application that process HTTP, TCP, and DNS health check requests and require access to event metadata across multiple operations within a single request lifecycle.

### Rules

- **R-CACHE-001** MUST: The cache layer MUST provide request-scoped isolation to prevent data leakage between concurrent requests.
- **R-CACHE-002** MUST: Implement cache Get operations at the beginning of handler functions to check for existing event data before performing expensive retrieval operations, and handle nil or empty return values appropriately.
- **R-CACHE-003** MUST: Use cache Set operations immediately after retrieving or computing event data from upstream sources to ensure subsequent operations within the same request can access the cached data.
- **R-CACHE-004** MUST: Establish and enforce a consistent cache key naming convention across all handler types to prevent collisions and improve code maintainability, documenting the convention in handler implementation guidelines.
- **R-CACHE-005** SHOULD: Create shared cache access utilities or interfaces that enforce consistent Get, Set, and Query patterns across all handler implementations.
- **R-CACHE-006** SHOULD: Implement monitoring for memory usage patterns correlated with request volume to detect potential memory leaks from cache entries not properly cleaned up after request completion.

### Verify

```bash
# Discover the project's test suite location and execute handler tests that verify cache Get operations return expected values for known cache keys
# Discover the project's test suite location and execute handler tests that verify cache Set operations persist data accessible by subsequent Get operations within the same request context
# Discover the project's static analysis or linting configuration and execute checks that verify cache operations follow established key naming conventions
```

**Accept when:**
- All handler tests pass, demonstrating that cache Get and Set operations correctly store and retrieve event data within request lifecycles
- Static analysis confirms that cache key names follow the established naming convention across all handler implementations
- Integration tests verify that handlers using cache operations exhibit reduced latency compared to direct data retrieval approaches
- Code review confirms cache layer usage prevents data leakage between concurrent requests

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing handler code that bypasses the cache layer for event data retrieval are rejected during code review. Test failures indicating incorrect cache operation usage block merge to main branch. Static analysis violations for cache key naming conventions trigger build failures.
</enforcement>