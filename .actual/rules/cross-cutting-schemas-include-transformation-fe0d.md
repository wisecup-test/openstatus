# Enforce Zod Schema Validation for All Protected Procedure Inputs: Schemas Include Transformation

These rules are ALWAYS ACTIVE for all tRPC protected procedures that accept external input parameters, including public API endpoints exposed through router definitions and procedures that construct database queries using input parameters.

### Rules

- **R-ZOD-001** MUST: All protected procedures that accept input parameters include schema validation declarations at procedure boundaries before business logic execution.
- **R-ZOD-002** MUST: Schema validation occurs before any query builder methods are invoked to prevent partially-validated parameters from reaching SQL construction.
- **R-ZOD-003** MAY: Schemas MAY include transformation rules such as coercion, normalization, or default values when the transformation preserves semantic meaning.
- **R-ZOD-004** MUST: Enum-based parameters define enum constants in a shared schema module and reference them in both validation schemas and database schema definitions to maintain consistency.
- **R-ZOD-005** MUST: Schema validation tests verify both that valid inputs are accepted and invalid inputs are rejected with appropriate error messages.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules to verify schema validation coverage
grep -r "test" package.json | head -5

# Locate the project's static analysis configuration and execute the type checker to confirm all procedure input parameters have corresponding schema declarations
find . -name "tsconfig.json" -o -name ".eslintrc*" | head -5

# Search the codebase for protected procedures and verify each procedure that accepts input parameters includes a schema validation declaration before business logic execution
grep -r "protectedProcedure\|publicProcedure" --include="*.ts" --include="*.tsx" | grep -v node_modules | head -20

# Verify no database queries are constructed using unvalidated input parameters
grep -r "where\|filter" --include="*.ts" --include="*.tsx" | grep -v "z\." | grep -v node_modules | head -20
```

**Accept when:**
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs
- Enum constants are defined in shared schema modules and referenced consistently across validation and database schemas
- Schema transformation rules preserve semantic meaning and do not introduce type coercion vulnerabilities

<enforcement>
Claude Code MUST NOT skip or defer verification. All protected procedures must be audited for schema validation presence before merging. Static analysis in CI must detect procedures accepting input without schema declarations.
</enforcement>