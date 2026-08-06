# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: State Stores Provide

These rules are ALWAYS ACTIVE for all HTTP route handlers receiving external platform interaction webhooks, state management operations for temporary interaction data with expiration policies, platform SDK API calls for updating conversation UI elements and thread messages, and validation logic for inbound platform payloads and cached state schemas.

### Rules

- **R-ASYNC-001** SHOULD: State stores should provide separate namespaces for action state and thread-to-action mappings to support concurrent interactions within the same conversation thread.
- **R-ASYNC-002** MUST: All integration handler functions use async signatures and await all cache operations and platform API calls.
- **R-ASYNC-003** MUST: State store implementations provide atomic consume operations that delete state after retrieval.
- **R-ASYNC-004** MUST: Error handling in handlers triggers compensating platform API calls to update UI with failure messages.
- **R-ASYNC-005** MUST: Wrap all async handler entry points in try-catch blocks and implement global unhandled rejection handlers.
- **R-ASYNC-006** MUST: Define state store interfaces with put, get, consume, and findByThread operations; implement both cache-backed and memory-backed variants for production and testing.
- **R-ASYNC-007** MUST: Structure handlers with clear phases: parse and validate inbound payload, retrieve cached state, execute business logic, update platform UI, consume state atomically.
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

# Verify all route handlers use async signatures
grep -r "async.*function\|async.*=>" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i "handler\|route" | wc -l

# Verify state store implementations have atomic consume operations
grep -r "consume\|delete\|remove" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i "state\|store" | head -10

# Verify error handling with compensating API calls
grep -r "catch\|error.*=>\|compensat" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread
- All async handler entry points are wrapped in try-catch blocks with global unhandled rejection handlers registered
- State store interfaces define put, get, consume, and findByThread operations with both cache-backed and memory-backed implementations
- Handlers follow the clear phase structure: parse/validate, retrieve cached state, execute logic, update platform UI, consume state atomically
- Cache expiration policies are configured and monitored for effectiveness
- Structured error logging includes thread identifiers, user identifiers, and action types

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code affecting external platform integration handlers and state management operations.
</enforcement>