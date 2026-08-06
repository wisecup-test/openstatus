# Enforce Zod Schema Validation for All Protected Procedure Inputs: Protected Procedures That

These rules are ALWAYS ACTIVE for all protected procedures that accept external input parameters, including tRPC protected procedures, public API endpoints exposed through router definitions, and procedures that construct database queries using input parameters.

### Rules

- **R-ZVAL-001** MUST: All protected procedures that accept input parameters MUST declare a schema using the validation library detected in the project dependencies before executing any business logic or database queries.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules to verify schema validation coverage
grep -r "test" package.json | head -5

# Locate the project's static analysis configuration and execute the type checker to confirm all procedure input parameters have corresponding schema declarations
find . -name "tsconfig.json" -o -name ".eslintrc*" | head -3

# Search the codebase for protected procedures and verify each procedure that accepts input parameters includes a schema validation declaration before business logic execution
grep -r "protectedProcedure" --include="*.ts" --include="*.tsx" | grep -v "node_modules" | head -10
```

**Accept when:**
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review checklist requires reviewers to verify schema validation is present for all new protected procedures. Static analysis in continuous integration pipeline detects procedures that accept input parameters without schema declarations. Integration tests validate that invalid input to protected procedures returns validation errors rather than database errors.
</enforcement>