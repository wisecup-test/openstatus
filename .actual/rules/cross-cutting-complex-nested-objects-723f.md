# Enforce Zod Schema Validation for All Protected Procedure Inputs: Complex Nested Objects

These rules are ALWAYS ACTIVE for all tRPC protected procedures that accept external input parameters, public API endpoints exposed through router definitions, procedures that construct database queries using input parameters, and endpoints that perform workspace-scoped authorization checks.

### Rules

- **R-ZOD-001** SHOULD: Complex nested objects SHOULD decompose validation into reusable schema fragments that can be composed across multiple procedures.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules to verify schema validation coverage
npm test -- --testPathPattern=router

# Locate the project's static analysis configuration and execute the type checker to confirm all procedure input parameters have corresponding schema declarations
npm run type-check

# Search the codebase for protected procedure definitions and verify each procedure that accepts input parameters includes a schema validation declaration before business logic execution
grep -r "protectedProcedure\|publicProcedure" --include="*.ts" --include="*.tsx" | grep -v "schema:"
```

**Accept when:**
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs
- Complex nested validation schemas are decomposed into reusable fragments and composed across multiple procedures

<enforcement>
Claude Code MUST NOT skip or defer verification. All protected procedures must be audited for schema validation presence before merging. Static analysis in CI must detect procedures accepting input without schema declarations.
</enforcement>