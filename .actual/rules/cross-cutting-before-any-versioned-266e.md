# Adopt Chat API Update Pattern for Ephemeral Message State Management: Before Any Versioned

These rules are ALWAYS ACTIVE for all chat platform integrations that use interactive message components, asynchronous workflows that update users on operation progress or completion, error handling flows that communicate failures through existing messages, and confirmation and cancellation interactions that modify message state.

### Rules

- **R-CHAT-001** MUST: Before using any versioned chat platform client library, discover the project's dependency lock artifact, resolve the exact installed version, and verify all API methods against that version's official documentation.
- **R-CHAT-002** MUST: Store message coordinates (channel ID and timestamp) immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- **R-CHAT-003** MUST: Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- **R-CHAT-004** MUST: Use the chat update API with channel and timestamp parameters for all message state transitions in interactive flows, and clear interactive blocks in terminal states.
- **R-CHAT-005** SHOULD: When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.
- **R-CHAT-006** SHOULD: Implement retry logic with exponential backoff for message updates, log update failures for monitoring, and consider fallback mechanisms such as posting new messages when updates fail after retries.
- **R-CHAT-007** SHOULD: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures that may indicate deleted messages.
- **R-CHAT-008** SHOULD: Design interaction flows to minimize concurrent updates to the same message, use optimistic locking or versioning if the chat platform supports it, and ensure update operations are idempotent where possible.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'package-lock.json' -o -name 'Gemfile' -o -name 'Gemfile.lock' -o -name 'requirements.txt' -o -name 'poetry.lock' -o -name 'go.mod' -o -name 'go.sum' | head -5

# 2. Identify the build tool and inspect the lock artifact for exact resolved version
grep -E '"version"|version =' $(find . -name '*lock*' -o -name 'Gemfile.lock' | head -1) | grep -i chat | head -3

# 3. Locate the project's test suite and run integration tests
find . -path '*/test*' -name '*chat*' -o -path '*/spec*' -name '*chat*' | head -5

# 4. Locate static analysis or linting configuration
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.rubocop.yml' -o -name 'golangci.yml' | head -3

# 5. Verify message update patterns in integration code
grep -r 'chat\.update\|message.*update\|channel.*timestamp' --include='*.js' --include='*.py' --include='*.rb' --include='*.go' . | grep -v node_modules | head -10

# 6. Verify error handling around message update calls
grep -r 'try\|catch\|except\|rescue' --include='*.js' --include='*.py' --include='*.rb' --include='*.go' . | grep -A 3 'chat\.update' | head -10

# 7. Verify message coordinates are persisted
grep -r 'channel.*timestamp\|message.*id\|coordinate' --include='*.js' --include='*.py' --include='*.rb' --include='*.go' . | grep -v node_modules | head -10
```

**Accept when:**
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.
- The exact version of the chat platform client library is verified against the project's lock artifact before any API methods are used.
- Integration tests pass that verify message state transitions occur correctly across success and error paths.
- Static analysis rules flag chat API usage without corresponding error handling or coordinate persistence.

<enforcement>
Claude Code MUST NOT skip or defer verification of message update patterns, error handling, coordinate persistence, and version grounding before approving chat platform integration code.
</enforcement>