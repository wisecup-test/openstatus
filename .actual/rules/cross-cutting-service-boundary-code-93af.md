# Standardize Service Boundary Definitions Through Header and Context Accessors: Service Boundary Code

These rules are ALWAYS ACTIVE for all HTTP request and response handlers in monitoring services, external client initialization and request preparation code, context value management for event tracking and request correlation, and query parameter extraction for handler configuration.

### Rules

- **R-SBC-001** SHOULD: Service boundary code SHOULD set standard headers including User-Agent and Content-Type using header setter methods.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' | head -1

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|tcp|dns|handler|boundary)' | head -5

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING*' -o -name 'CODE_REVIEW*' -o -name '.github/PULL_REQUEST_TEMPLATE*' | head -1

# Search for direct field access patterns at service boundaries (anti-pattern)
grep -r 'request\.' --include='*.js' --include='*.ts' --include='*.go' --include='*.py' | grep -v 'request\.get\|request\.set\|request\.header' | head -10

# Verify framework accessor method usage for headers
grep -r 'setHeader\|set-header\|header(' --include='*.js' --include='*.ts' --include='*.go' --include='*.py' | head -10
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points
- Standard headers (User-Agent, Content-Type) are set via setter methods in all boundary crossing points

<enforcement>
Claude Code MUST NOT skip or defer verification. All boundary accessor patterns must be confirmed before accepting changes to service boundary code.
</enforcement>