# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: Handlers Separate Concerns

These rules are ALWAYS ACTIVE for all HTTP route handlers receiving external platform interaction webhooks, state management operations for temporary interaction data with expiration policies, platform SDK API calls for updating conversation UI elements and thread messages, and validation logic for inbound platform payloads and cached state schemas.

### Rules

- **R-ASYNC-001** MUST: Handlers must separate concerns into distinct asynchronous operations: state retrieval from cache layer, payload validation, business logic execution, and platform API updates.
- **R-ASYNC-002** MUST: All integration handler functions use async signatures and await all cache operations and platform API calls.
- **R-ASYNC-003** MUST: State store implementations provide atomic consume operations that delete state after retrieval.
- **R-ASYNC-004** MUST: Error handling in handlers triggers compensating platform API calls to update UI with failure messages.
- **R-ASYNC-005** MUST: Wrap all async handler entry points in try-catch blocks and implement global unhandled rejection handlers.
- **R-ASYNC-006** SHOULD: Define state store interfaces with put, get, consume, and findByThread operations; implement both cache-backed and memory-backed variants for production and testing.
- **R-ASYNC-007** SHOULD: Configure cache expiration policies based on expected user interaction latency and monitor expired action rates to detect if TTL is too aggressive.
- **R-ASYNC-008** SHOULD: Implement structured error logging with context including thread identifiers, user identifiers, and action types to support debugging of asynchronous failures.

### Verify

```bash
# Discover the project's integration test suite and execute tests covering concurrent interaction handling with cache-backed state stores
find . -type f -name '*test*' -o -name '*spec*' | grep -i integrat | head -5

# Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
find . -type f \( -name '.eslintrc*' -o -name 'tsconfig.json' -o -name '.pylintrc' -o -name 'ruff.toml' \) | head -5

# Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup
grep -r "unhandledRejection\|process.on\|addEventListener" --include="*.js" --include="*.ts" --include="*.py" . | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread
- Static analysis rules enforce async function signatures for route handlers
- Unhandled promise rejection handlers are registered at application startup

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before code is committed.
</enforcement>