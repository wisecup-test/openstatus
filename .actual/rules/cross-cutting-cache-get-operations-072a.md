# Adopt Cache Layer for Event Data in Handler Functions: Cache Get Operations

These rules are ALWAYS ACTIVE for all handler functions in the checker application that process HTTP, TCP, and DNS health check requests and require access to event metadata across multiple operations within a single request lifecycle.

### Rules

- **R-CACHE-001** SHOULD: Cache Get operations SHOULD return nil or empty values when the requested key does not exist, allowing handlers to distinguish between cache hits and misses.

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

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST follow the cache Get operation pattern with nil/empty return value handling. Code review MUST verify cache Get and Set patterns in new handler implementations. Static analysis violations for cache key naming conventions trigger build failures. Pull requests introducing handler code that bypasses the cache layer for event data retrieval are rejected during code review.
</enforcement>