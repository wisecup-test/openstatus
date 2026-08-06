# Standardize Service Boundary Definitions Through Header and Context Accessors: Custom Header Propagation

These rules are ALWAYS ACTIVE for all HTTP request and response handlers in monitoring services, external client initialization and request preparation code, context value management for event tracking and request correlation, and query parameter extraction for handler configuration.

### Rules

- **R-SBD-001** SHOULD: Custom header propagation SHOULD iterate over header collections and apply values using the same accessor pattern.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' | head -1

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*boundary*' | head -5

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING.md' -o -name 'CODE_REVIEW.md' -o -name '.github/PULL_REQUEST_TEMPLATE.md' | head -1
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points

<enforcement>
Clause Code MUST NOT skip or defer verification. All boundary accessor patterns must be validated before merge.
</enforcement>