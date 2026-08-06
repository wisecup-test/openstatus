# Log Message Queue Interactions at Service Boundaries: Logging Statements Include

These rules are ALWAYS ACTIVE for all components that interact with message queue boundaries, including message queue consumers, action execution handlers, cache-backed confirmation stores, and tool renderers invoked via message-driven patterns.

### Rules

- **R-MQ-LOG-001** SHOULD: Logging statements SHOULD include a consistent prefix or namespace identifier to distinguish message queue boundary logs from other application logs.

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
- Logging statements use consistent prefixes or namespace identifiers across all message queue boundary interactions.

<enforcement>
Clause Code MUST NOT skip or defer verification. All new message queue consumers require logging at consumption boundaries with consistent prefix identifiers. Pull requests introducing message queue interactions without logging are flagged during code review and require revision.
</enforcement>