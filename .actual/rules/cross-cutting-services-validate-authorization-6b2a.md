# Standardize Query-Based Data Access with Authorization Headers for Primary Datastores: Services Validate Authorization

These rules are ALWAYS ACTIVE for all service boundary code that accesses primary datastores, including client abstractions, middleware layers, API routers, and endpoints that coordinate datastore queries.

### Rules

- **R-SVCS-001** MUST: Services MUST validate authorization credentials via middleware before processing requests that access primary datastores.

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

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis in continuous integration pipelines MUST scan for direct datastore SDK imports without authorization wrappers. Code review checklists MUST require verification of authorization middleware and query validation for all new datastore access code. Integration test coverage MUST ensure all datastore-backed endpoints have authorization failure test cases.
</enforcement>