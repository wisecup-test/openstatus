# Validate External Search Parameters Through Dedicated Cache Parser: Endpoints Redirect Alternative

These rules are ALWAYS ACTIVE for all public API page endpoints that accept search parameters from external clients as Promise-wrapped objects requiring asynchronous resolution before use.

### Rules

- **R-CACHE-001** MAY: Endpoints MAY redirect to alternative routes when required search parameters fail validation or are absent.
- **R-CACHE-002** MUST: All public API page endpoints that accept search parameters from external clients validate parameters through the dedicated search parameter cache parser before any data prefetching or routing logic.
- **R-CACHE-003** MUST: Asynchronous page functions that receive searchParams as Promise-wrapped props await Promise resolution and immediately pass the result to the cache parser before any other operations.
- **R-CACHE-004** MUST: Validation must complete before data operations begin to prevent invalid input from propagating into downstream operations.
- **R-CACHE-005** SHOULD: For endpoints that conditionally redirect based on parameter presence, perform validation first and then check the parsed result for required fields rather than checking the raw parameter object.

### Verify

```bash
# Discover the project's static analysis or linting configuration and execute the verification script
# that checks public endpoint functions for search parameter validation calls
find . -name '.eslintrc*' -o -name 'tsconfig.json' -o -name '.prettierrc*' | head -1

# Locate the test suite for the search parameter cache parser and execute tests
# to confirm validation rules are enforced
find . -path '*/test*' -name '*cache*parser*' -o -path '*/test*' -name '*search*param*' | head -5

# Identify the project's endpoint scanning tool and run it to generate a report
# of all public page functions, then verify each one includes validation before data operations
grep -r 'searchParams' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -E '(page\.|export.*default)' | head -20

# Verify cache parser is called before data operations
grep -r 'searchParamsCache\.parse' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' -B 2 -A 5
```

**Accept when:**
- All public API page endpoints that accept search parameters include a call to the cache parser before any data prefetching or routing logic
- Static analysis or automated scanning confirms no public endpoint uses search parameter values without prior validation
- Test coverage for the search parameter cache parser includes validation of all parameter types accepted by public endpoints
- Promise-wrapped searchParams are awaited before being passed to the cache parser
- Conditional redirects check the parsed validation result rather than raw parameter objects

<enforcement>
Claude Code MUST NOT skip or defer verification. All public endpoints accepting external search parameters MUST be validated through the dedicated cache parser before any downstream operations. Build failures and pull request blocks are mandatory when validation is missing.
</enforcement>