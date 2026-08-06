# Standardize Service Boundary Definitions Through Header and Context Accessors: Implementations Cache Frequently

These rules are ALWAYS ACTIVE for all HTTP request and response handlers in monitoring services, external client initialization and request preparation code, context value management for event tracking and request correlation, and query parameter extraction for handler configuration.

### Rules

- **R-SBD-001** MAY: Implementations MAY cache frequently accessed context values in local variables after initial retrieval.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.lint*' -o -name 'eslint*' -o -name 'pylint*' -o -name '.flake8' | head -5

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*boundary*' | head -10

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING*' -o -name 'CODE_REVIEW*' -o -name '.github/PULL_REQUEST_TEMPLATE*' | head -5
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points
- Context values are cached in local variables only after initial retrieval through framework accessors

<enforcement>
Clause Code MUST NOT skip or defer verification. All boundary accessor patterns must be confirmed through static analysis, integration testing, and code review before acceptance.
</enforcement>