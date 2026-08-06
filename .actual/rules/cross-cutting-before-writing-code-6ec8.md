# Enforce Rate Limiting for Workflow Monitoring Operations: Before Writing Code

These rules are ALWAYS ACTIVE for all workflow monitoring operations that interact with external task scheduling services or perform user-scoped workflow initialization.

### Rules

- **R-RATELIMIT-001** MUST: Before writing code that instantiates rate limiting components from versioned libraries, discover the project's dependency lock artifact, resolve the exact installed version, and verify API compatibility against that version's official documentation.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact, then identify the rate limiting library and its resolved version
find . -name "package-lock.json" -o -name "yarn.lock" -o -name "Pipfile.lock" -o -name "go.sum" | head -1

# Locate workflow monitoring module source and verify rate limiter instantiation with explicit tokensPerInterval and interval configuration
grep -r "tokensPerInterval" --include="*.js" --include="*.ts" --include="*.py" . | grep -i "workflow\|monitor"

# Identify the project's test execution mechanism and run integration tests covering workflow initialization with rate limiting enabled
grep -r "test\|spec" package.json setup.py pyproject.toml 2>/dev/null | head -5
```

**Accept when:**
- Rate limiter instantiation is present in workflow monitoring code with explicit tokensPerInterval and interval parameters matching external service quota documentation
- Integration tests demonstrate that bulk workflow initialization operations respect configured rate limits and do not exceed external service quotas
- Code review confirms rate limiting is applied before all external task scheduling service invocations in user enumeration and workflow initialization paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Rate limiter instantiation must be verified against the exact resolved dependency version before any code is written.
</enforcement>