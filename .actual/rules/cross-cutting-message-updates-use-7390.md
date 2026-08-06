# Adopt Chat API Update Pattern for Ephemeral Message State Management: Message Updates Use

These rules are ALWAYS ACTIVE for all chat platform integrations that use interactive message components, asynchronous workflows that update users on operation progress or completion, error handling flows that communicate failures through existing messages, and confirmation and cancellation interactions that modify message state.

### Rules

- **R-CHAT-001** SHOULD: Message updates should use semantic prefixes or emoji indicators to visually distinguish state types such as errors, cancellations, and expirations.
- **R-CHAT-002** MUST: Store message coordinates (channel ID and timestamp) immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- **R-CHAT-003** MUST: Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- **R-CHAT-004** MUST: Clear interactive blocks during terminal state transitions to prevent users from interacting with stale actions while preserving the message history and context within the conversation thread.
- **R-CHAT-005** SHOULD: When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.
- **R-CHAT-006** MUST: Implement retry logic with exponential backoff for message update API calls to handle transient failures due to network issues or rate limiting.
- **R-CHAT-007** MUST: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures.
- **R-CHAT-008** SHOULD: Design interaction flows to minimize concurrent updates to the same message, and ensure update operations are idempotent where possible.

### Verify

```bash
# Discover the project's test suite location and execute integration tests
# that verify message update operations across error, cancellation, and expiration scenarios.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(chat|message|integration)' | head -5

# Locate the project's static analysis or linting configuration
# and run checks that enforce error handling around message update API calls.
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'setup.cfg' \) | head -5

# Identify the project's dependency verification tooling and confirm
# that the chat platform client library version matches the lock artifact.
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -3

# Search for message update API usage patterns in the codebase.
grep -r 'chat\.update\|message.*update\|channel.*timestamp' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -10

# Verify error handling wraps action execution.
grep -r 'try\|catch\|except' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -i 'update\|message' | head -10
```

**Accept when:**
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.
- Retry logic with exponential backoff is implemented for message update API calls.
- Message update failures are logged and monitored for detection of patterns indicating deleted messages or permission issues.
- Interaction flows are designed to minimize concurrent updates to the same message.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for chat platform integrations within scope. Code review and integration tests MUST verify compliance before merge.
</enforcement>