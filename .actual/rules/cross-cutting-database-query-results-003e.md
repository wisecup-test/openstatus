# Enforce Schema Validation for Workspace Data Access: Database Query Results

These rules are ALWAYS ACTIVE for all database query operations that return workspace entities, data structures passed to cache layers, entity objects submitted to external task queues or workflow orchestration, and data retrieved from user or workspace tables that participate in rate-limited operations.

### Rules

- **R-SCHEMA-001** MUST: All database query results that represent workspace entities MUST be validated against a declared schema before being used in business logic, cache operations, or external service calls.

### Verify

```bash
# Locate the project's test execution configuration and run the test suite covering workspace data access patterns
# to verify schema validation is applied consistently
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Discover the project's static analysis or linting configuration and execute checks that detect database query
# patterns without corresponding validation
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name '.rubocop.yml' | head -1

# Identify the project's type checking configuration and verify that workspace entity types are enforced
# at data access boundaries
find . -name 'tsconfig.json' -o -name 'mypy.ini' -o -name '.pyre_configuration' | head -1
```

**Accept when:**
- All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached
- Validation failures produce explicit error types that are handled appropriately at each access point
- Schema definitions are colocated with or directly derived from database table definitions to prevent drift
- Test coverage includes validation failure scenarios for each workspace data access point

<enforcement>
Claude Code MUST NOT skip or defer verification. All database query results representing workspace entities MUST be validated against a declared schema before use. Violations block pull requests until validation is added. Approved exceptions require architecture review, performance profiling data, and documented compensating controls.
</enforcement>