# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Error Conditions Encountered

These rules are ALWAYS ACTIVE for HTTP clients instantiated for external endpoint monitoring, health-check handlers that evaluate HTTP assertions, ping and region availability verification workflows, and integration test scenarios that exercise HTTP request/response cycles.

### Rules

- **R-RETRY-001** MUST: Error conditions encountered during retry operations must be logged with contextual information including error messages and operation identifiers.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool,
# then locate and inspect the lock or resolution artifact to determine exact
# resolved versions of retry and HTTP client dependencies.

# Discover the project's test execution tooling and run integration test suites
# that exercise HTTP client retry behavior with simulated transient failures
# and timeout conditions.

# Discover the project's static analysis or linting configuration and verify
# that HTTP client instantiation sites include exponential backoff retry
# wrappers with configurable retry counts.
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing HTTP clients without retry mechanisms are blocked until retry logic is added. Existing violations are tracked as technical debt items and prioritized based on endpoint criticality and failure rate history. Production incidents involving unhandled transient failures trigger immediate remediation and retry logic addition.
</enforcement>