# Adopt Chat API Update Pattern for Ephemeral Message State Management: Services Persist Message

These rules are ALWAYS ACTIVE for all chat platform integrations that use interactive message components, asynchronous workflows that update users on operation progress or completion, error handling flows that communicate failures through existing messages, and confirmation and cancellation interactions that modify message state.

### Rules

- **R-CHAT-001** MUST: Services must persist message coordinates (channel identifier and timestamp) from initial message creation to enable subsequent updates throughout the interaction lifecycle.
- **R-CHAT-002** MUST: Store message coordinates immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- **R-CHAT-003** MUST: Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- **R-CHAT-004** MUST: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures that may indicate deleted messages.
- **R-CHAT-005** SHOULD: When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.
- **R-CHAT-006** SHOULD: Implement retry logic with exponential backoff for message updates, log update failures for monitoring, and consider fallback mechanisms such as posting new messages when updates fail after retries.
- **R-CHAT-007** SHOULD: Design interaction flows to minimize concurrent updates to the same message, use optimistic locking or versioning if the chat platform supports it, and ensure update operations are idempotent where possible.

### Verify

```bash
# Discover the project's test suite location and execute integration tests
# that verify message update operations across error, cancellation, and expiration scenarios.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(chat|message|integration)' | head -5

# Locate the project's static analysis or linting configuration and run checks
# that enforce error handling around message update API calls.
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'setup.cfg' \) | head -5

# Identify the project's dependency verification tooling and confirm that
# the chat platform client library version matches the lock artifact.
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -3

# Search for message update API usage patterns in the codebase.
grep -r 'chat\.update\|message.*update\|channel.*timestamp' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -10

# Verify error handling wraps action execution in integration code.
grep -r 'try\|catch\|except' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -i 'message\|chat\|update' | head -10
```

**Accept when:**
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.
- Integration tests verify message update operations across error, cancellation, and expiration scenarios.
- Static analysis rules enforce error handling around message update API calls.
- Chat platform client library version is verified against the lock artifact and all used API methods are documented for that version.

<enforcement>
Claude Code MUST NOT skip or defer verification of message coordinate persistence, error handling patterns, and message update API usage. All R-CHAT rules are mandatory for chat integrations in scope.
</enforcement>