# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Http Client Timeout

These rules are ALWAYS ACTIVE for all HTTP client instantiation in handler and job execution code paths that perform external service communication, including health checks, monitoring, assertion evaluation, and region ping operations.

### Rules

- **R-HTTP-TIMEOUT-001** SHOULD: HTTP client timeout values SHOULD be coordinated with retry backoff maximum elapsed time to prevent timeout-retry interaction deadlocks.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit Timeout field configuration
# (Exact command depends on project's build tool and linter configuration — consult project repository)

# Discover and execute the project's integration test suite to verify timeout behavior under simulated external service latency scenarios
# (Exact command depends on project's test framework — consult project repository)

# Discover and execute the project's configuration validation to ensure timeout parameters are present in all required handler and job contexts
# (Exact command depends on project's configuration validation tool — consult project repository)
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit Timeout field configuration derived from parameters or constants
- No HTTP client instantiation uses default zero-value timeout or omits Timeout field
- Integration tests demonstrate timeout enforcement under simulated unresponsive external service conditions
- Timeout values are not duplicated as magic numbers across files; they are extracted from configuration constants or request structures
- Maximum total elapsed time (retry count × per-attempt timeout) remains within acceptable service-level bounds

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until HTTP client timeout configuration is added. Code review rejection is required for any external service communication lacking explicit timeout boundaries. Post-deployment monitoring alerts on timeout error rate anomalies trigger incident review.
</enforcement>