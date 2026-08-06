# Standardize Service Boundary Definitions Through Header and Context Accessors: Context Value Storage

These rules are ALWAYS ACTIVE for all HTTP request and response handlers in monitoring services, external client initialization and request preparation code, context value management for event tracking and request correlation, and query parameter extraction for handler configuration.

### Rules

- **R-BOUNDARY-001** MUST: Context value storage and retrieval at service boundaries MUST use framework context accessor methods with string keys.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.lint*' -o -name 'eslint*' -o -name 'pylint*' -o -name 'golangci*' | head -5

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*boundary*' | head -10

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING*' -o -name 'CODE_REVIEW*' -o -name '.github/PULL_REQUEST_TEMPLATE*'

# Search for direct field access patterns at service boundaries (anti-pattern)
grep -r '\bRequest\.' . --include='*.go' --include='*.js' --include='*.ts' --include='*.py' | grep -v 'accessor\|getter\|setter' | head -20
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types (HTTP, TCP, DNS) demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points
- Context key constants are defined in a central location to prevent typos and key mismatches

<enforcement>
Claude Code MUST NOT skip or defer verification. All boundary accessor patterns must be confirmed through code review, static analysis, and integration test execution before accepting changes.
</enforcement>