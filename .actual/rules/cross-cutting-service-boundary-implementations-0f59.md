# Standardize Service Boundary Definitions Through Header and Context Accessors: Service Boundary Implementations

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers, external client initialization code, context value management for event tracking, and all other service boundary implementations across the checker application.

### Rules

- **R-SB-001** MUST: All service boundary implementations MUST use framework-provided accessor methods to read and write request headers rather than direct field access.

### Verify

```bash
# Discover and execute the project's static analysis or linting configuration
# to verify that boundary accessor patterns are used consistently
find . -name '.lint*' -o -name 'eslint*' -o -name 'golangci*' -o -name 'pylint*' | head -5

# Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers
# to confirm accessor method behavior
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*boundary*' | head -10

# Identify the project's code review checklist or guidelines
find . -name 'CONTRIBUTING*' -o -name 'CODE_REVIEW*' -o -name '.github/PULL_REQUEST_TEMPLATE*' | head -5

# Search for direct field access patterns at service boundaries (anti-pattern)
grep -r '\bRequest\.' . --include='*.go' --include='*.ts' --include='*.js' --include='*.py' 2>/dev/null | grep -v 'accessor\|getter\|setter' | head -20

# Verify accessor method usage in handler implementations
grep -r 'Header(\|GetHeader\|SetHeader\|Context(\|WithValue' . --include='*.go' --include='*.ts' --include='*.js' --include='*.py' 2>/dev/null | wc -l
```

**Accept when:**
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types (HTTP, TCP, DNS) demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points
- Code review checklist includes boundary accessor pattern requirements and is enforced during pull request approval

<enforcement>
Claude Code MUST NOT skip or defer verification. All service boundary implementations MUST be reviewed against R-SB-001 before acceptance. Direct field access at service boundaries is a violation and must be rejected with guidance to use framework accessors.
</enforcement>