# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Handler Level Http

These rules are ALWAYS ACTIVE for all HTTP client instantiation in handler and job execution contexts that perform external service communication, including health checks, monitoring, assertion evaluation, and region ping operations.

### Rules

- **R-HTTP-001** SHOULD: Handler-level HTTP clients SHOULD use request-specific timeout parameters when available, falling back to service-level defaults.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit Timeout field configuration
# Discover and execute the project's integration test suite to verify timeout behavior under simulated external service latency scenarios
# Discover and execute the project's configuration validation to ensure timeout parameters are present in all required handler and job contexts
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit Timeout field configuration derived from parameters or constants
- No HTTP client instantiation uses default zero-value timeout or omits Timeout field
- Integration tests demonstrate timeout enforcement under simulated unresponsive external service conditions

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until HTTP client timeout configuration is added. Code review rejection required for any external service communication lacking explicit timeout boundaries.
</enforcement>