# Enforce Schema Validation for Workspace Data Access: Schema Definitions Colocated

These rules are ALWAYS ACTIVE for all database query results representing workspace entities, data structures passed to cache layers, entity objects submitted to external task queues or workflow orchestration, and data retrieved from user or workspace tables that participate in rate-limited operations.

### Rules

- **R-SCHEMA-001** SHOULD: Schema definitions SHOULD be colocated with database table definitions to maintain consistency between persistence layer and validation layer.
- **R-SCHEMA-002** MUST: All database queries returning workspace entities include explicit schema validation before data is used in business logic or cached.
- **R-SCHEMA-003** MUST: Validation failures produce explicit error types that are handled appropriately at each access point.
- **R-SCHEMA-004** MUST: Schema definitions are colocated with or directly derived from database table definitions to prevent drift.
- **R-SCHEMA-005** MAY: Performance-critical hot paths may be exempted from validation overhead where validation overhead is measured and documented as unacceptable, and static type guarantees are proven sufficient (EXC-001).

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
- Static analysis confirms no unvalidated database query patterns exist in workspace data access code
- Workspace entity types are enforced at all data access boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification. All workspace data access patterns MUST include schema validation. Violations are tracked as technical debt with priority based on data criticality and failure risk. Exception requests require performance profiling data and architecture review.
</enforcement>