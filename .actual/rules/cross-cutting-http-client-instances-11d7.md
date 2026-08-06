# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Http Client Instances

These rules are ALWAYS ACTIVE for all HTTP client instantiation in handler and job execution code paths that perform external service communication.

### Rules

- **R-HTTP-001** MUST: All HTTP client instances used for external service communication MUST configure an explicit Timeout field at instantiation time.

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