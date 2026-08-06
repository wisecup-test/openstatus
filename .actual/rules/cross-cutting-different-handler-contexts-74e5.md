# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Different Handler Contexts

These rules are ALWAYS ACTIVE for all HTTP client instantiation in handler and job execution contexts that perform external service communication, including health checks, monitoring, assertion evaluation, and region ping operations.

### Rules

- **R-HTTP-TIMEOUT-001** MUST: All HTTP client instantiations for external service communication include explicit Timeout field configuration.
- **R-HTTP-TIMEOUT-002** MUST: No HTTP client instantiation uses default zero-value timeout or omits the Timeout field.
- **R-HTTP-TIMEOUT-003** MAY: Different handler contexts MAY configure different timeout thresholds based on expected service response characteristics.
- **R-HTTP-TIMEOUT-004** SHOULD: Timeout configuration parameters should be derived from request structures or configuration constants, not duplicated as magic numbers across files.
- **R-HTTP-TIMEOUT-005** SHOULD: Timeout values should be reviewed in conjunction with retry backoff configuration to ensure maximum total elapsed time remains within acceptable service-level bounds.
- **R-HTTP-TIMEOUT-006** SHOULD: Observability instrumentation should track timeout error rates per handler context to enable data-driven timeout tuning decisions.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit Timeout field configuration
# (Exact command to be derived from project repository)

# Discover and execute the project's integration test suite to verify timeout behavior under simulated external service latency scenarios
# (Exact command to be derived from project repository)

# Discover and execute the project's configuration validation to ensure timeout parameters are present in all required handler and job contexts
# (Exact command to be derived from project repository)
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit Timeout field configuration derived from parameters or constants
- No HTTP client instantiation uses default zero-value timeout or omits Timeout field
- Integration tests demonstrate timeout enforcement under simulated unresponsive external service conditions
- Timeout values are not duplicated as magic numbers across files
- Maximum total elapsed time (timeout × retry attempts) remains within acceptable service-level bounds

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until HTTP client timeout configuration is added. Code review rejection is required for any external service communication lacking explicit timeout boundaries. Post-deployment monitoring alerts on timeout error rate anomalies trigger incident review.
</enforcement>