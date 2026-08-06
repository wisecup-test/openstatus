# Log Message Queue Interactions at Service Boundaries: Implementations Use Structured

These rules are ALWAYS ACTIVE for all components that interact with message queue boundaries, including message queue consumers, action execution handlers, cache-backed confirmation stores, and tool renderers invoked via message-driven patterns.

### Rules

- **R-MQ-001** MAY: Implementations MAY use structured logging formats to capture message metadata, timestamps, and correlation identifiers as separate fields rather than concatenated strings.
- **R-MQ-002** MUST: All message queue consumption points include logging statements that capture operation outcomes and error details.
- **R-MQ-003** MUST: Sensitive data in message payloads MUST be sanitized before logging to prevent exposure of user data or credentials.
- **R-MQ-004** SHOULD: Consider wrapping message queue operations in a logging decorator or middleware that automatically captures operation metadata, timing, and outcomes to ensure consistent logging across all consumers.
- **R-MQ-005** SHOULD: When logging validation failures, include both the validation error structure and a sanitized representation of the payload to enable debugging without exposing sensitive data.

### Verify

```bash
# Discover the project's test runner and execute the test suite covering message queue interaction modules
# to verify logging is present at consumption boundaries.

# Search the codebase for message queue consumption patterns and verify each has associated error logging
# with contextual information.

# Review log output from integration tests involving message queue workflows to confirm diagnostic context
# is sufficient for failure correlation.
```

**Accept when:**
- All message queue consumption points include logging statements that capture operation outcomes and error details.
- Test coverage demonstrates that validation failures and execution errors at message queue boundaries produce logs with sufficient diagnostic context.
- Code review confirms that sensitive data in message payloads is sanitized before logging.
- Logging templates or wrapper functions enforce consistent format, prefixes, and metadata inclusion across all message queue boundaries.

<enforcement>
Clause Code MUST NOT skip or defer verification. Pull requests introducing message queue consumers without logging are flagged during code review and require revision. Periodic audits of message queue interaction code identify gaps in logging coverage for remediation.
</enforcement>