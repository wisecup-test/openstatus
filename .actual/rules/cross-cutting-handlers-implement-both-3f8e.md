# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: Handlers Implement Both

These rules are ALWAYS ACTIVE for all HTTP route handlers receiving external platform interaction webhooks, state management operations for temporary interaction data with expiration policies, platform SDK API calls for updating conversation UI elements and thread messages, and validation logic for inbound platform payloads and cached state schemas.

### Rules

- **R-ASYNC-001** SHOULD: Handlers should implement both production cache-backed and development memory-backed state stores with identical interfaces to support testing without external dependencies.
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
find . -type f -name '*test*' -o -name '*spec*' | grep -i integrat | head -5

# Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'tsconfig.json' -o -name '.prettierrc*' \) | head -5

# Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup
grep -r "unhandledRejection\|process.on" --include="*.js" --include="*.ts" | head -10

# Verify all route handlers use async signatures
grep -r "(req.*res.*=>\|function.*req.*res" --include="*.js" --include="*.ts" | grep -v async | head -10

# Verify state store implementations have atomic consume operations
grep -r "consume\|delete" --include="*.js" --include="*.ts" | grep -i state | head -10

# Verify error handling with compensating API calls
grep -r "catch\|error" --include="*.js" --include="*.ts" | grep -i "platform\|api\|update" | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread
- Both cache-backed and memory-backed state store implementations exist with identical interfaces
- Unhandled promise rejection handlers are registered at application startup
- Structured error logging includes thread identifiers, user identifiers, and action types

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before code is committed. Rules marked SHOULD represent strong preferences that should be followed unless explicitly justified and documented.
</enforcement>