# Adopt Chat API Update Pattern for Ephemeral Message State Management: Database Update Operations

These rules are ALWAYS ACTIVE for all chat platform integrations that use interactive message components, asynchronous workflows that update users on operation progress or completion, error handling flows that communicate failures through existing messages, and confirmation and cancellation interactions that modify message state.

### Rules

- **R-CHAT-001** SHOULD: Database update operations that persist state changes should be coordinated with message updates to maintain consistency between stored state and user-visible messages.
- **R-CHAT-002** MUST: Store message coordinates (channel ID and timestamp) immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- **R-CHAT-003** MUST: Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- **R-CHAT-004** SHOULD: When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.
- **R-CHAT-005** MUST: Implement retry logic with exponential backoff for message updates, log update failures for monitoring, and consider fallback mechanisms such as posting new messages when updates fail after retries.
- **R-CHAT-006** MUST: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures that may indicate deleted messages.
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

# Search for message update API calls in the codebase
grep -r 'chat\.update\|message.*update\|channel.*timestamp' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -10

# Verify error handling patterns around message updates
grep -r 'try\|catch\|except' --include='*.js' --include='*.ts' --include='*.py' . | grep -A 5 -B 5 'update' | head -20
```

**Accept when:**
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.
- Retry logic with exponential backoff is implemented for message update failures.
- Message update calls are wrapped in error handling that gracefully degrades when message references are invalid.
- Interaction flows are designed to minimize concurrent updates to the same message.

<enforcement>
Claude Code MUST NOT skip or defer verification. All message update operations MUST include error handling, coordinate persistence, and database coordination. Pull requests that introduce chat integrations without proper message update patterns MUST be blocked until corrected.
</enforcement>