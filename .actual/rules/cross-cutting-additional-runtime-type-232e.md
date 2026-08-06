# Enforce Schema Validation for Workspace Data Access: Additional Runtime Type

These rules are ALWAYS ACTIVE for all database query operations, cache layer interactions, and external service integrations that handle workspace entity data.

### Rules

- **R-SCHEMA-001** MAY: Additional runtime type guards MAY be applied at service boundaries where data crosses process or network boundaries.
- **R-SCHEMA-002** MUST: All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached.
- **R-SCHEMA-003** MUST: Validation failures produce explicit error types that are handled appropriately at each access point.
- **R-SCHEMA-004** MUST: Schema definitions are colocated with or directly derived from database table definitions to prevent drift.
- **R-SCHEMA-005** SHOULD: Validated data is cached rather than raw query results, preventing validation overhead on cache hits while maintaining data integrity guarantees.
- **R-SCHEMA-006** SHOULD: Validation logic is implemented as a thin wrapper around database query operations, ensuring validation occurs before any business logic processes the data.

### Verify

```bash
# Locate the project's test execution configuration and run the test suite covering workspace data access patterns
# to verify schema validation is applied consistently
echo "Running workspace data access validation tests..."

# Discover the project's static analysis or linting configuration and execute checks that detect
# database query patterns without corresponding validation
echo "Scanning for unvalidated database query patterns..."

# Identify the project's type checking configuration and verify that workspace entity types
# are enforced at data access boundaries
echo "Verifying workspace entity type enforcement..."
```

**Accept when:**
- All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached
- Validation failures produce explicit error types that are handled appropriately at each access point
- Schema definitions are colocated with or directly derived from database table definitions to prevent drift
- Test coverage includes validation failure scenarios for each workspace data access point
- Static analysis detects and flags database query operations without corresponding validation calls
- Workspace entity types are enforced at data access boundaries through type checking

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace data access patterns MUST include schema validation at system boundaries. Violations are tracked as technical debt and blocked in pull requests until remediated.
</enforcement>