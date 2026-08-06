# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: Temporary Interaction State

These rules are ALWAYS ACTIVE for all HTTP route handlers receiving external platform interaction webhooks, state management operations for temporary interaction data with expiration policies, platform SDK API calls for updating conversation UI elements and thread messages, and validation logic for inbound platform payloads and cached state schemas.

### Rules

- **R-ASYNC-001** MUST: Temporary interaction state must be stored in a cache layer with time-to-live expiration and consumed atomically after successful processing to prevent duplicate handling.
- **R-ASYNC-002** MUST: All integration handler functions use async signatures and await all cache operations and platform API calls.
- **R-ASYNC-003** MUST: State store implementations provide atomic consume operations that delete state after retrieval.
- **R-ASYNC-004** MUST: Error handling in handlers triggers compensating platform API calls to update UI with failure messages.
- **R-ASYNC-005** MUST: Define state store interfaces with put, get, consume, and findByThread operations; implement both cache-backed and memory-backed variants for production and testing.
- **R-ASYNC-006** MUST: Structure handlers with clear phases: parse and validate inbound payload, retrieve cached state, execute business logic, update platform UI, consume state atomically.
- **R-ASYNC-007** MUST: Wrap all async handler entry points in try-catch blocks and implement global unhandled rejection handlers.
- **R-ASYNC-008** SHOULD: Configure cache expiration policies based on expected user interaction latency and monitor expired action rates to detect if TTL is too aggressive.
- **R-ASYNC-009** SHOULD: Implement structured error logging with context including thread identifiers, user identifiers, and action types to support debugging of asynchronous failures.

### Verify

```bash
# Discover the project's integration test suite and execute tests covering concurrent interaction handling with cache-backed state stores
find . -type f -name '*test*' -o -name '*spec*' | grep -i integration | head -5

# Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'tsconfig.json' -o -name '.pylintrc' \) | head -5

# Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup
grep -r "unhandledRejection\|process.on\|addEventListener" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i "rejection\|error" | head -10

# Verify all route handlers receiving platform webhooks use async signatures
grep -r "router\.post\|router\.put\|app\.post\|app\.put" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v "async" | head -10

# Verify state store implementations have atomic consume operations
grep -r "consume\|delete\|remove" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i "state\|cache" | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread
- All async handler entry points are wrapped in try-catch blocks
- Global unhandled promise rejection handlers are registered at application startup
- Cache expiration policies are configured and monitored
- Structured error logging includes thread identifiers, user identifiers, and action types

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before code is committed. Static analysis checks must pass, code review must confirm error handling and compensating API calls, and integration tests must cover concurrent interaction scenarios.
</enforcement>