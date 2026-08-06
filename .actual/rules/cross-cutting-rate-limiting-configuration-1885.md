# Enforce Rate Limiting for Workflow Monitoring Operations: Rate Limiting Configuration

These rules are ALWAYS ACTIVE for all workflow monitoring operations that interact with external task scheduling services or perform user-scoped workflow initialization.

### Rules

- **R-RATE-001** SHOULD: Rate limiting configuration values SHOULD be derived from environment-specific settings or centralized configuration to enable tuning across deployment environments without code changes.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact, then identify the rate limiting library and its resolved version
find . -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.xml' -o -name 'build.gradle' | head -5

# Locate workflow monitoring module source and verify rate limiter instantiation with explicit tokensPerInterval and interval configuration
grep -r "tokensPerInterval" --include="*.js" --include="*.ts" --include="*.py" --include="*.java" .

# Identify the project's test execution mechanism and run integration tests covering workflow initialization with rate limiting enabled
grep -r "test\|spec" package.json 2>/dev/null || grep -r "test" pom.xml 2>/dev/null || find . -name '*test*.js' -o -name '*spec*.js' | head -5
```

**Accept when:**
- Rate limiter instantiation is present in workflow monitoring code with explicit tokensPerInterval and interval parameters matching external service quota documentation
- Integration tests demonstrate that bulk workflow initialization operations respect configured rate limits and do not exceed external service quotas
- Code review confirms rate limiting is applied before all external task scheduling service invocations in user enumeration and workflow initialization paths
- Rate limiter configuration values are sourced from environment-specific settings or centralized configuration rather than hardcoded in source

<enforcement>
Claude Code MUST NOT skip or defer verification. Rate limiter instantiation and configuration sourcing MUST be confirmed before accepting changes to workflow monitoring operations that interact with external task scheduling services.
</enforcement>