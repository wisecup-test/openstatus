# Adopt Chat API Update Pattern for Ephemeral Message State Management: Message State Updates

These rules are ALWAYS ACTIVE for all chat platform integrations that use interactive message components, asynchronous workflows that update users on operation progress or completion, error handling flows that communicate failures through existing messages, and confirmation and cancellation interactions that modify message state.

### Rules

- **R-MSG-001** MUST: All message state updates for ephemeral interactions must use the chat update API with channel identifier and message timestamp to modify messages in place.
- **R-MSG-002** MUST: Store message coordinates immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- **R-MSG-003** MUST: Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- **R-MSG-004** SHOULD: When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.
- **R-MSG-005** SHOULD: Implement retry logic with exponential backoff for message updates, log update failures for monitoring, and consider fallback mechanisms such as posting new messages when updates fail after retries.
- **R-MSG-006** SHOULD: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures that may indicate deleted messages.
- **R-MSG-007** SHOULD: Design interaction flows to minimize concurrent updates to the same message, use optimistic locking or versioning if the chat platform supports it, and ensure update operations are idempotent where possible.

### Verify

```bash
# Discover the project's test suite location and execute integration tests
# that verify message update operations across error, cancellation, and expiration scenarios.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(message|chat|update)' | head -20

# Locate the project's static analysis or linting configuration and run checks
# that enforce error handling around message update API calls.
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'setup.cfg' \) | head -10

# Identify the project's dependency verification tooling and confirm that
# the chat platform client library version matches the lock artifact.
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -5

# Search for message update API calls in the codebase
grep -r 'chat\.update\|message.*update\|update.*message' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -20

# Verify error handling patterns around message updates
grep -r 'try\|catch\|except' --include='*.js' --include='*.ts' --include='*.py' . | grep -A 5 -B 5 'update' | head -30
```

**Accept when:**
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.
- Integration tests verify message update operations across error, cancellation, and expiration scenarios.
- Static analysis rules enforce error handling around message update API calls.
- Chat platform client library version is verified against the lock artifact and all used API methods are documented for that version.

<enforcement>
Claude Code MUST NOT skip or defer verification. All message state update implementations MUST be reviewed against R-MSG-001 through R-MSG-007. Integration tests MUST pass before accepting changes. Static analysis MUST confirm error handling patterns are in place.
</enforcement>