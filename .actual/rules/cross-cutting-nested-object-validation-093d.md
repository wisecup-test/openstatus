# Enforce Zod Schema Validation at Integration Boundaries: Nested Object Validation

These rules are ALWAYS ACTIVE for all integration endpoints and service boundaries that accept external input, including HTTP API endpoints, message queue consumers, webhook handlers, cache deserialization, and service-to-service integration points.

### Rules

- **R-NESTED-001** SHOULD: Nested object validation SHOULD use composed schema definitions to enforce consistency across related data structures.

### Verify

```bash
# Discover the project's test runner from the dependency manifest and execute the integration test suite covering validation behavior at external boundaries

# Discover the project's static analysis configuration and execute type checking to verify schema definitions provide type inference for validated payloads

# Discover the project's linting configuration and execute validation to ensure schema definitions are colocated with integration endpoint handlers
```

**Accept when:**
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code
- Schema validators are defined as constants colocated with endpoint handlers to maintain clear visibility of validation contracts
- Safe parsing methods that return result types are used for validation errors to enable graceful error handling without exception-based control flow

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration endpoints accepting external input MUST have explicit schema validators applied before business logic execution. Validation failures MUST be logged with structured error details and sanitized before external exposure.
</enforcement>