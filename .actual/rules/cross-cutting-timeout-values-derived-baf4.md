# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Timeout Values Derived

These rules are ALWAYS ACTIVE for all HTTP client instantiation in handler and job execution code paths that perform external service communication.

### Rules

- **R-HTTP-TIMEOUT-001** MUST: Timeout values MUST be derived from request-scoped configuration parameters or system-wide constants, never hardcoded literals scattered across implementation files.
- **R-HTTP-TIMEOUT-002** MUST: All HTTP client instantiations for external service health checks MUST include explicit Timeout field configuration.
- **R-HTTP-TIMEOUT-003** MUST: All HTTP client instantiations for monitoring job execution MUST include explicit Timeout field configuration.
- **R-HTTP-TIMEOUT-004** MUST: All HTTP client instantiations for assertion evaluation against external endpoints MUST include explicit Timeout field configuration.
- **R-HTTP-TIMEOUT-005** MUST: All HTTP client instantiations for region ping operations MUST include explicit Timeout field configuration.
- **R-HTTP-TIMEOUT-006** MUST: No HTTP client instantiation for external service communication MAY use default zero-value timeout or omit Timeout field.
- **R-HTTP-TIMEOUT-007** SHOULD: Timeout values SHOULD be reviewed in conjunction with retry backoff configuration to ensure maximum total elapsed time remains within acceptable service-level bounds.
- **R-HTTP-TIMEOUT-008** SHOULD: Observability instrumentation SHOULD be added to track timeout error rates per handler context, enabling data-driven timeout tuning decisions.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit Timeout field configuration
# (Command discovery required from project repository)

# Discover and execute the project's integration test suite to verify timeout behavior under simulated external service latency scenarios
# (Command discovery required from project repository)

# Discover and execute the project's configuration validation to ensure timeout parameters are present in all required handler and job contexts
# (Command discovery required from project repository)
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit Timeout field configuration derived from parameters or constants.
- No HTTP client instantiation uses default zero-value timeout or omits Timeout field.
- Integration tests demonstrate timeout enforcement under simulated unresponsive external service conditions.
- Timeout values are not duplicated as magic numbers across files.
- Maximum total elapsed time (timeout × retry attempts) remains within acceptable service-level bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until HTTP client timeout configuration is added. Code review rejection is required for any external service communication lacking explicit timeout boundaries. Post-deployment monitoring alerts on timeout error rate anomalies trigger incident review.
</enforcement>