# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: Platform Calls Updates

These rules are ALWAYS ACTIVE for all HTTP route handlers receiving external platform interaction webhooks, state management operations for temporary interaction data with expiration policies, platform SDK API calls for updating conversation UI elements and thread messages, and validation logic for inbound platform payloads and cached state schemas.

### Rules

- **R-PLATFORM-CALLS-001** MUST: Platform API calls for UI updates must be awaited and error responses must trigger compensating updates to inform users of failure states.

### Verify

```bash
# Discover the project's integration test suite and execute tests covering concurrent interaction handling with cache-backed state stores
find . -type f -name '*test*' -o -name '*spec*' | grep -i integration | head -5

# Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'tsconfig.json' -o -name '.jshintrc' \) | head -5

# Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup
grep -r "unhandledRejection\|rejectionHandled" . --include="*.js" --include="*.ts" --include="*.mjs" 2>/dev/null | head -10
```

**Accept when:**
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread

<enforcement>
Claude Code MUST NOT skip or defer verification. All four acceptance criteria must be confirmed before approving changes to integration handlers.
</enforcement>