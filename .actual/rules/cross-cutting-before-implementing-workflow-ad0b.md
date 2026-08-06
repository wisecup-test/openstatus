# Enforce Workflow Step Contracts for Distributed Cron Monitoring: Before Implementing Workflow

These rules are ALWAYS ACTIVE for all workflow orchestration components that coordinate distributed monitoring tasks across temporal boundaries, including step transition logic, task queue submission interfaces, and rate-limited workflow execution paths.

### Rules

- **R-WORKFLOW-001** MUST: Before implementing workflow contracts that depend on versioned cloud task queue libraries, database schema libraries, or email notification libraries, discover the project's dependency lock artifact, resolve the exact installed version, and verify all contract-related APIs exist in that version's official documentation.

- **R-WORKFLOW-002** MUST: Define workflow step contracts as exported types that specify input state structure, output state structure, temporal progression rules, and failure recovery semantics, co-located with workflow orchestration logic for discoverability.

- **R-WORKFLOW-003** MUST: Implement contract validation at workflow initialization and step transition boundaries using schema parsing to catch state inconsistencies before they propagate to cache layers or task queues.

- **R-WORKFLOW-004** MUST: Log contract validation failures with sufficient context for debugging distributed coordination issues.

- **R-WORKFLOW-005** SHOULD: Design cache key patterns and expiration policies as part of the public contract surface when cache consistency is critical to workflow reliability.

- **R-WORKFLOW-006** SHOULD: Document the relationship between cache TTL values and workflow temporal boundaries to prevent premature state eviction.

- **R-WORKFLOW-007** SHOULD: Separate workflow contracts into initialization contracts (LaunchMonitorWorkflow), step transition contracts (Step3Days, Step14Days, StepPaused), and completion contracts to enable independent testing and evolution of each workflow stage.

### Verify

```bash
# Discover the project's workflow contract type definitions and verify that all step transition types export explicit input state, output state, and temporal progression semantics
grep -r "LaunchMonitorWorkflow\|Step14Days\|Step3Days\|StepPaused" --include="*.ts" --include="*.js" .

# Locate the project's workflow orchestration test suite and execute tests that verify contract validation occurs at initialization and step transition boundaries
find . -name "*.test.ts" -o -name "*.test.js" | xargs grep -l "workflow.*contract\|step.*transition" | head -5

# Identify the project's cache layer integration and verify that cache key patterns and expiration policies referenced in workflow contracts match actual cache operations
grep -r "cache.*key\|TTL\|expiration" --include="*.ts" --include="*.js" . | grep -i workflow

# Verify dependency lock artifact exists and contains versioned libraries
ls -la package-lock.json yarn.lock pnpm-lock.yaml 2>/dev/null | head -1
```

**Accept when:**
- All workflow step contracts define explicit types for state transitions and temporal progression, verified by successful compilation without type errors
- Contract validation logic executes at workflow initialization and step boundaries, verified by test coverage showing validation failures prevent invalid state transitions
- Cache operations use key patterns and expiration policies consistent with workflow contract specifications, verified by integration tests that exercise full workflow lifecycle
- Dependency lock artifact is present and exact versions of cloud task queue, database schema, and email notification libraries are resolvable and documented

<enforcement>
Claude Code MUST NOT skip or defer verification. Type system verification during compilation, integration tests exercising full workflow lifecycle, and code review checklist verification are mandatory before approving workflow implementations. Compilation failures block deployment when workflow code violates contract type signatures. Runtime contract validation failures trigger workflow suspension and alert on-call engineers.
</enforcement>