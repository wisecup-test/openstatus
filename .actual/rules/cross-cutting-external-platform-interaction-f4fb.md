# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: External Platform Interaction

These rules are ALWAYS ACTIVE for all external platform interaction handlers, HTTP route handlers receiving platform webhooks, state management operations for temporary interaction data, platform SDK API calls, and validation logic for inbound platform payloads.

### Rules

- **R-EX-001** MUST: All external platform interaction handlers must implement asynchronous function signatures to support non-blocking execution of state retrieval, validation, and platform API calls.
- **R-EX-002** MUST: State store implementations must provide atomic consume operations that delete state after retrieval to prevent duplicate processing.
- **R-EX-003** MUST: All cache operations and platform API calls within handlers must be awaited.
- **R-EX-004** MUST: Error handling in handlers must trigger compensating platform API calls to update UI with failure messages.
- **R-EX-005** MUST: All async handler entry points must be wrapped in try-catch blocks with structured error logging including thread identifiers, user identifiers, and action types.
- **R-EX-006** SHOULD: State store interfaces should define put, get, consume, and findByThread operations with both cache-backed and memory-backed variants for production and testing.
- **R-EX-007** SHOULD: Cache expiration policies should be configured based on expected user interaction latency with monitoring of expired action rates.
- **R-EX-008** SHOULD: Handlers should be structured with clear phases: parse and validate inbound payload, retrieve cached state, execute business logic, update platform UI, consume state atomically.

### Verify

```bash
# Discover the project's integration test suite and execute tests covering concurrent interaction handling with cache-backed state stores
find . -type f -name '*test*' -o -name '*spec*' | grep -i integration | head -5

# Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
find . -type f \( -name '.eslintrc*' -o -name 'tsconfig.json' -o -name '.pylintrc' -o -name 'ruff.toml' \) | head -5

# Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup
grep -r "unhandledRejection\|process.on\|addEventListener" --include="*.ts" --include="*.js" --include="*.py" . | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread
- All async handler entry points are wrapped in try-catch blocks with structured error logging
- Unhandled promise rejection handlers are registered at application startup

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before code is committed. Static analysis checks must pass, code review must confirm error handling and compensating API calls, and integration tests must cover concurrent interaction scenarios.
</enforcement>