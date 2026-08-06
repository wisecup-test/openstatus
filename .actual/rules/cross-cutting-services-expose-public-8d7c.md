# Standardize Query-Based Data Access with Authorization Headers for Primary Datastores: Services Expose Public

These rules are ALWAYS ACTIVE for all service boundary code that accesses primary datastores, including client abstractions, middleware layers, and API routers that coordinate datastore queries.

### Rules

- **R-DATASTORE-001** MUST: All datastore client code include authorization header injection with credentials sourced from runtime configuration.
- **R-DATASTORE-002** MUST: Authorization middleware validate token format and expiration before allowing requests to proceed to datastore query logic.
- **R-DATASTORE-003** MUST: Implement credential scrubbing in logging infrastructure to prevent authorization tokens from appearing in logs or error messages.
- **R-DATASTORE-004** SHOULD: Extract common patterns for query construction, header injection, and error handling into shared client abstractions.
- **R-DATASTORE-005** MAY: Services MAY expose public API contracts that abstract underlying datastore query patterns for external consumers.
- **R-DATASTORE-006** MUST NOT: Bypass client abstractions or authorization middleware with direct datastore SDK usage in production code.

### Verify

```bash
# Discover the project's dependency resolution artifact and identify exact versions
# of datastore client libraries in use across all services
find . -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'go.sum' -o -name 'Cargo.lock' | head -5

# Locate and execute static analysis to verify authorization headers are present
# in all datastore client instantiations
grep -r "authorization" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" | grep -i "header\|bearer\|token" | wc -l

# Identify direct datastore SDK imports that may bypass authorization
grep -r "import.*datastore" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" | grep -v "client\|wrapper\|abstraction"

# Find and run integration tests validating authorization middleware
find . -path "*/test*" -o -path "*/spec*" | grep -i "auth\|datastore" | head -10
```

**Accept when:**
- All datastore client code includes authorization header injection with credentials sourced from runtime configuration.
- Static analysis confirms no direct datastore SDK usage bypasses client abstractions or authorization middleware.
- Integration tests demonstrate that unauthorized requests to datastore-backed endpoints return 401 status codes without executing queries.
- Credential handling violations are not present in logging or error response bodies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All datastore access patterns MUST be validated against R-DATASTORE-001 through R-DATASTORE-006 before code is considered compliant.
</enforcement>