# Validate External Search Parameters Through Dedicated Cache Parser: Search Parameter Validation

These rules are ALWAYS ACTIVE for all public API page endpoints that accept search parameters from external clients, all asynchronous page functions that receive searchParams as Promise-wrapped props, all data prefetching operations that depend on search parameter values, and all routing decisions conditional on search parameter presence or validity.

### Rules

- **R-SEARCH-001** SHOULD: Search parameter validation logic SHOULD be centralized in a single cache parser instance to ensure consistent validation rules across all public endpoints.

### Verify

```bash
# Discover the project's static analysis or linting configuration and execute the verification script that checks public endpoint functions for search parameter validation calls

# Locate the test suite for the search parameter cache parser and execute tests to confirm validation rules are enforced

# Identify the project's endpoint scanning tool and run it to generate a report of all public page functions, then verify each one includes validation before data operations
```

**Accept when:**
- All public API page endpoints that accept search parameters include a call to the cache parser before any data prefetching or routing logic
- Static analysis or automated scanning confirms no public endpoint uses search parameter values without prior validation
- Test coverage for the search parameter cache parser includes validation of all parameter types accepted by public endpoints

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis MUST scan public endpoint functions for search parameter validation patterns during continuous integration. Code review MUST verify search parameter validation is present in any new or modified public endpoint. Security audit MUST include manual inspection of search parameter validation logic and testing with malicious input patterns. Build MUST fail if static analysis detects public endpoints that accept search parameters without validation.
</enforcement>