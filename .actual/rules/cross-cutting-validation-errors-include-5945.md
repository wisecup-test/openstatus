# Enforce Schema Validation for Workspace Data Access: Validation Errors Include

These rules are ALWAYS ACTIVE for all database query results representing workspace entities, data structures passed to cache layers, entity objects submitted to external task queues or workflow orchestration, and data retrieved from user or workspace tables that participate in rate-limited operations.

### Rules

- **R-SCHEMA-001** SHOULD: Validation errors SHOULD include sufficient context to identify the source query, affected entity type, and specific validation failure reason for debugging.

### Verify

```bash
# Locate the project's test execution configuration and run the test suite covering workspace data access patterns
# to verify schema validation is applied consistently
find . -name "*test*" -o -name "*spec*" | head -5

# Discover the project's static analysis or linting configuration and execute checks that detect database query
# patterns without corresponding validation
find . -name ".eslintrc*" -o -name "pylintrc" -o -name "tox.ini" -o -name "setup.cfg" | head -5

# Identify the project's type checking configuration and verify that workspace entity types are enforced
# at data access boundaries
find . -name "tsconfig.json" -o -name "mypy.ini" -o -name "pyproject.toml" | head -5
```

**Accept when:**
- All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached
- Validation failures produce explicit error types that are handled appropriately at each access point
- Schema definitions are colocated with or directly derived from database table definitions to prevent drift
- Test coverage includes validation failure scenarios for each workspace data access point
- Validation error messages include source query, entity type, and specific failure reason

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace data access points must be audited for schema validation compliance before accepting changes.
</enforcement>