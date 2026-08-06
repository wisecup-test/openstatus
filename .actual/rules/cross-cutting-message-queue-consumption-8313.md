# Log Message Queue Interactions at Service Boundaries: Message Queue Consumption

These rules are ALWAYS ACTIVE for all components that interact with message queue boundaries, including message queue consumers, action execution handlers, cache-backed confirmation stores, tool renderers, and result processors invoked via message-driven patterns.

### Rules

- **R-MQ-001** MUST: All message queue consumption points MUST log the operation outcome, including success confirmations and failure diagnostics with sufficient context to correlate the message to its producer.

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

<enforcement>
Clause Code MUST NOT skip or defer verification. Pull requests introducing message queue consumers without logging are flagged during code review and require revision. Incidents involving message queue failures without diagnostic logs trigger retrospectives to add missing logging. Periodic audits of message queue interaction code identify gaps in logging coverage for remediation.
</enforcement>