# Enforce Schema Validation for Workspace Data Access: Schema Validation Occur

These rules are ALWAYS ACTIVE for all database query results representing workspace entities, data structures passed to cache layers, entity objects submitted to external task queues or workflow orchestration, and data retrieved from user or workspace tables that participate in rate-limited operations.

### Rules

- **R-SCHEMA-001** MUST: Schema validation MUST occur immediately after data retrieval and before any transformation, caching, or propagation to downstream systems.
- **R-SCHEMA-002** MUST: All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached.
- **R-SCHEMA-003** MUST: Validation failures produce explicit error types that are handled appropriately at each access point.
- **R-SCHEMA-004** SHOULD: Schema definitions are colocated with or directly derived from database table definitions to prevent drift.
- **R-SCHEMA-005** SHOULD: Validated data is cached rather than raw query results, preventing validation overhead on cache hits while maintaining data integrity guarantees.

### Verify

```bash
# Locate the project's test execution configuration and run the test suite covering workspace data access patterns
# to verify schema validation is applied consistently
grep -r "workspace.*validation\|schema.*validation" test/ --include="*.js" --include="*.ts" --include="*.py"

# Discover the project's static analysis or linting configuration and execute checks that detect database query
# patterns without corresponding validation
grep -r "query\|select\|fetch" src/ --include="*.js" --include="*.ts" --include="*.py" | grep -v "validate\|schema\|assert"

# Identify the project's type checking configuration and verify that workspace entity types are enforced
# at data access boundaries
grep -r "workspace.*type\|entity.*type" src/ --include="*.ts" --include="*.d.ts"
```

**Accept when:**
- All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached
- Validation failures produce explicit error types that are handled appropriately at each access point
- Schema definitions are colocated with or directly derived from database table definitions to prevent drift
- Test coverage includes validation failure scenarios for each workspace data access point
- No database query patterns exist without corresponding validation calls

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace data access points MUST include schema validation before proceeding. Violations block pull requests until validation is added.
</enforcement>