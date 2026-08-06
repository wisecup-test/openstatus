# Enforce Schema Validation for Workspace Data Access: Validation Failures Result

These rules are ALWAYS ACTIVE for all database query results representing workspace entities, data structures passed to cache layers, entity objects submitted to external task queues or workflow orchestration, and data retrieved from user or workspace tables that participate in rate-limited operations.

### Rules

- **R-VALIDATION-001** MUST: Validation failures MUST result in explicit error handling that prevents invalid data from entering workflow state machines, cache layers, or task queues.
- **R-VALIDATION-002** MUST: All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached.
- **R-VALIDATION-003** MUST: Validation failures produce explicit error types that are handled appropriately at each access point.
- **R-VALIDATION-004** MUST: Schema definitions are colocated with or directly derived from database table definitions to prevent drift.
- **R-VALIDATION-005** SHOULD: Implement validation as a thin wrapper around database query operations, ensuring validation occurs before any business logic processes the data.
- **R-VALIDATION-006** SHOULD: For systems using cache layer patterns, ensure validated data is cached rather than raw query results, preventing validation overhead on cache hits while maintaining data integrity guarantees.

### Verify

```bash
# Locate the project's test execution configuration and run the test suite covering workspace data access patterns
# to verify schema validation is applied consistently
echo "Running workspace data access validation tests..."

# Discover the project's static analysis or linting configuration and execute checks that detect database query
# patterns without corresponding validation
echo "Scanning for unvalidated database query patterns..."

# Identify the project's type checking configuration and verify that workspace entity types are enforced
# at data access boundaries
echo "Verifying workspace entity type enforcement..."
```

**Accept when:**
- All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached
- Validation failures produce explicit error types that are handled appropriately at each access point
- Schema definitions are colocated with or directly derived from database table definitions to prevent drift
- Test coverage includes validation failure scenarios for each workspace data access point
- Static analysis detects no database query operations without corresponding validation calls
- Code review checklist confirms validation for all new database query patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace data access points must be audited for schema validation compliance before code is committed. Violations must be tracked and remediated according to the enforcement strategy defined in the ADR.
</enforcement>