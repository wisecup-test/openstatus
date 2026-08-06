# Log Message Queue Interactions at Service Boundaries: Validation Failures Deserialized

These rules are ALWAYS ACTIVE for all components that interact with message queue boundaries, including message queue consumers, action execution handlers, cache-backed confirmation stores, and tool renderers invoked via message-driven patterns.

### Rules

- **R-MQ-001** SHOULD: Validation failures on deserialized message payloads SHOULD log the validation error details and the raw or sanitized payload to aid debugging.

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
Clause Code MUST NOT skip or defer verification. All message queue consumption points must be audited for logging presence and diagnostic sufficiency before merge.
</enforcement>