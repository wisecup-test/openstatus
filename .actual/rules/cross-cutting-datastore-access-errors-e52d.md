# Standardize Query-Based Data Access with Authorization Headers for Primary Datastores: Datastore Access Errors

These rules are ALWAYS ACTIVE for all service boundary code that accesses primary datastores, including client abstractions, middleware layers, API routers, and endpoints that coordinate datastore queries.

### Rules

- **R-DATASTORE-001** MUST: Datastore access errors MUST be logged with contextual information using the service's logging infrastructure.

### Verify

```bash
# Discover the project's dependency resolution artifact and identify the exact versions of datastore client libraries in use across all services.
# Locate and execute the project's static analysis or linting configuration to verify that authorization headers are present in all datastore client instantiations.
# Identify and run the project's integration test suite that validates authorization middleware behavior and datastore access patterns.
```

**Accept when:**
- All datastore client code includes authorization header injection with credentials sourced from runtime configuration.
- Static analysis confirms no direct datastore SDK usage bypasses client abstractions or authorization middleware.
- Integration tests demonstrate that unauthorized requests to datastore-backed endpoints return 401 status codes without executing queries.
- Datastore access errors are logged with sufficient context for debugging and audit trails.

<enforcement>
Claude Code MUST NOT skip or defer verification. All datastore access error logging MUST be validated before accepting changes.
</enforcement>