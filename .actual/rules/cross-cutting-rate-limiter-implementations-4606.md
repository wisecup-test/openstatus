# Enforce Rate Limiting for Workflow Monitoring Operations: Rate Limiter Implementations

These rules are ALWAYS ACTIVE for all workflow monitoring operations that interact with external task scheduling services or perform user-scoped workflow initialization.

### Rules

- **R-RATELIMIT-001** MAY: Rate limiter implementations MAY use token bucket, sliding window, or fixed window algorithms based on the granularity requirements of the external service's quota enforcement.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact, then identify the rate limiting library and its resolved version
find . -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.xml' -o -name 'build.gradle' | head -5

# Locate workflow monitoring module source and verify rate limiter instantiation with explicit tokensPerInterval and interval configuration
grep -r "RateLimiter\|tokensPerInterval\|interval.*second" --include="*.js" --include="*.ts" --include="*.java" --include="*.py" | grep -i "workflow\|monitor"

# Identify the project's test execution mechanism and run integration tests covering workflow initialization with rate limiting enabled
grep -r "test\|spec" package.json build.gradle pom.xml 2>/dev/null | head -3
```

**Accept when:**
- Rate limiter instantiation is present in workflow monitoring code with explicit tokensPerInterval and interval parameters matching external service quota documentation
- Integration tests demonstrate that bulk workflow initialization operations respect configured rate limits and do not exceed external service quotas
- Code review confirms rate limiting is applied before all external task scheduling service invocations in user enumeration and workflow initialization paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Rate limiter instantiation must be confirmed in workflow monitoring modules before external service calls, and integration test coverage must validate quota compliance.
</enforcement>