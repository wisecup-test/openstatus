# Enforce Zod Schema Validation for All Protected Procedure Inputs: Before Any Validation

These rules are ALWAYS ACTIVE for all tRPC protected procedures that accept external input parameters, public API endpoints exposed through router definitions, procedures that construct database queries using input parameters, and endpoints that perform workspace-scoped authorization checks.

### Rules

- **R-ZOD-001** MUST: Before using any validation library API, locate the project's dependency lock artifact, resolve the exact installed version, and verify all schema methods against that version's official documentation.
- **R-ZOD-002** MUST: All tRPC protected procedures that accept input parameters include schema validation declarations at procedure boundaries before any business logic execution.
- **R-ZOD-003** MUST: Schema validation must occur before any database query builder methods are invoked to prevent partially-validated parameters from reaching SQL construction.
- **R-ZOD-004** MUST: When adding new enum-based parameters, define enum constants in a shared schema module and reference them in both validation schemas and database schema definitions.
- **R-ZOD-005** SHOULD: Implement error transformation middleware that sanitizes validation errors before returning them to API consumers, removing internal field names and constraint details.
- **R-ZOD-006** SHOULD: Include schema validation tests in integration test suites that verify schemas accept all valid production input patterns and reject invalid inputs.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules
# to verify schema validation coverage
npm test -- --testPathPattern=router

# Locate the project's static analysis configuration and execute the type checker to confirm all procedure input
# parameters have corresponding schema declarations
npm run type-check

# Search the codebase for protected procedures and verify each procedure that accepts input parameters includes
# a schema validation declaration before business logic execution
grep -r "protectedProcedure" --include="*.ts" --include="*.tsx" | grep -v "schema"
```

**Accept when:**
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs
- Error transformation middleware sanitizes validation errors before returning to API consumers
- Enum constants are centralized in shared schema modules and referenced consistently

<enforcement>
Claude Code MUST NOT skip or defer verification. All protected procedures must be audited for schema validation presence before merging. Static analysis in CI must detect and block procedures without schema declarations.
</enforcement>