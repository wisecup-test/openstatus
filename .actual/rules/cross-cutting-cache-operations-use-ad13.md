# Adopt Cache Layer for Event Data in Handler Functions: Cache Operations Use

These rules are ALWAYS ACTIVE for all handler functions in the checker application that process HTTP, TCP, and DNS health check requests and access event metadata through cache operations.

### Rules

- **R-CACHE-001** MUST: Cache operations MUST use string keys that identify the data type being cached.

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
Claude Code MUST NOT skip or defer verification. All cache operations in handler functions must comply with R-CACHE-001. Pull requests introducing handler code that violates this rule are rejected during code review. Test failures indicating incorrect cache operation usage block merge to main branch. Static analysis violations for cache key naming conventions trigger build failures.
</enforcement>