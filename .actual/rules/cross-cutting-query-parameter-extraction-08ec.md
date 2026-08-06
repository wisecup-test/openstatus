# Standardize Service Boundary Definitions Through Header and Context Accessors: Query Parameter Extraction

These rules are ALWAYS ACTIVE for all HTTP request and response handlers in monitoring services, external client initialization code, context value management for event tracking, and query parameter extraction for handler configuration.

### Rules

- **R-BOUNDARY-001** MUST: Query parameter extraction MUST use framework-provided query accessor methods rather than manual URL parsing.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.lint*' -o -name 'eslint*' -o -name 'pylint*' -o -name '.flake8' | head -5

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*handler*' | grep -E '\.(test|spec)\.(js|ts|py|go)$' | head -10

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING*' -o -name 'CODE_REVIEW*' -o -name '.github/PULL_REQUEST_TEMPLATE*' | head -5

# Search for direct field access patterns at service boundaries (anti-pattern)
grep -r 'request\.url' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' 2>/dev/null | head -10
grep -r 'response\.headers\[' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' 2>/dev/null | head -10
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points
- Code review checklist includes boundary accessor pattern requirements

<enforcement>
Claude Code MUST NOT skip or defer verification. All boundary accessor patterns MUST be validated before approving changes to service boundary code.
</enforcement>