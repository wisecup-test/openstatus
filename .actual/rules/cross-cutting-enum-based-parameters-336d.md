# Enforce Zod Schema Validation for All Protected Procedure Inputs: Enum Based Parameters

These rules are ALWAYS ACTIVE for all tRPC protected procedures that accept external input parameters, particularly those with enum-based parameters that construct database queries or perform workspace-scoped authorization checks.

### Rules

- **R-ENUM-001** MUST: Enum-based parameters MUST use schema enum validation to restrict values to the declared set of allowed constants.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules to verify schema validation coverage
grep -r "test" package.json | head -5

# Locate the project's static analysis configuration and execute the type checker to confirm all procedure input parameters have corresponding schema declarations
find . -name "tsconfig.json" -o -name ".eslintrc*" | head -3

# Search the codebase for protected procedures and verify each procedure that accepts input parameters includes a schema validation declaration before business logic execution
grep -r "protectedProcedure" --include="*.ts" --include="*.tsx" | grep -v node_modules | head -10
grep -r "z\.enum" --include="*.ts" --include="*.tsx" | grep -v node_modules | wc -l
```

**Accept when:**
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs
- Enum-based parameters are validated using schema enum declarations before any query builder methods are invoked

<enforcement>
Claude Code MUST NOT skip or defer verification. All protected procedures accepting enum-based parameters MUST include schema validation at procedure boundaries before database query construction.
</enforcement>