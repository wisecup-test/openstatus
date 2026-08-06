# Enforce Rate Limiting for Workflow Monitoring Operations: Workflow Monitoring Operations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all workflow monitoring operations that interact with external task scheduling services or perform user-scoped workflow initialization.

## Context

- Workflow monitoring systems coordinate user-scoped initialization tasks with external task scheduling services, creating potential for burst traffic patterns that exceed service quotas.
- The codebase integrates with cloud-based task queue services that enforce strict rate limits on API operations, requiring client-side throttling to prevent service disruptions.
- User workflow data is cached in distributed storage with time-based expiration policies, and concurrent access patterns during initialization can amplify request rates to downstream services.
- The system processes workflow steps across multiple time horizons with distinct lifecycle states, each triggering potential external service calls that must be coordinated within rate boundaries.

## Problem Statement

Without explicit rate limiting controls, workflow monitoring operations that enumerate users, initialize workflow state, and coordinate with external task scheduling services can exceed API quotas during bulk processing or system recovery scenarios, leading to service throttling, failed task creation, and degraded reliability for time-sensitive monitoring workflows.

## Decision

1. MUST: All workflow monitoring operations that interact with external task scheduling services MUST enforce client-side rate limiting with a configured tokens-per-interval budget.

## Policy Block

- MUST All workflow monitoring operations that interact with external task scheduling services MUST enforce client-side rate limiting with a configured tokens-per-interval budget.

In scope:
- Workflow monitoring cron jobs that enumerate users and initialize workflow state
- Operations that create or update tasks in external cloud-based task scheduling services
- Bulk processing routines that iterate over user collections and trigger per-user external service calls
- System recovery or backfill operations that may process large user cohorts concurrently

Out of scope:
- Internal database query operations that do not interact with external rate-limited services
- Cache read operations against distributed storage layers
- Workflow step execution logic that operates on already-initialized state
- User-facing API endpoints with separate rate limiting policies

Exceptions:
- EXC-001: Emergency recovery operations require temporary quota increases to restore service within defined RTO/RPO targets

## Rationale

- The evidence shows instantiation of a RateLimiter with tokensPerInterval: 15 and interval: 'second' in workflow monitoring code that coordinates with external task scheduling services, indicating explicit client-side throttling to respect service quotas.
- The codebase integrates with cloud task queue services that enforce strict API rate limits, and the observed rate limiting pattern prevents quota exhaustion during bulk user enumeration and workflow initialization operations.
- Workflow monitoring operates across multiple lifecycle states with time-based expiration policies in distributed cache layers, creating burst traffic patterns that require rate limiting to maintain reliability during concurrent processing.
- The pattern appears in cron-scheduled monitoring code that processes user collections and triggers external service calls, a context where uncontrolled request rates would directly impact system reliability and monitoring coverage.

## Consequences

Positive:
- Prevents service throttling and failed task creation by enforcing client-side rate limits that respect external service quotas
- Enables reliable bulk processing and system recovery operations without exceeding API rate boundaries
- Provides explicit control over request pacing, making system behavior predictable during high-load scenarios
- Reduces operational incidents related to quota exhaustion and improves overall monitoring reliability

Negative:
- Introduces latency for bulk operations as requests are throttled to stay within configured rate limits
- Requires careful tuning of rate limiter parameters to balance throughput with quota compliance across deployment environments
- Adds complexity to workflow initialization logic with additional rate limiting state management
- May require monitoring and alerting on rate limiter queue depth to detect capacity constraints

## Alternatives

- Rely on external service's server-side rate limiting and implement exponential backoff retry logic (rejected)
  Rejected because: Server-side rate limiting triggers after quota exhaustion, causing failed requests and degraded reliability. Reactive retry logic increases latency and complexity compared to proactive client-side throttling.
  When valid: Acceptable for low-volume operations where occasional rate limit errors do not impact SLOs, or when external service provides generous retry quotas separate from primary rate limits.
- Implement distributed rate limiting using shared state in cache layer to coordinate across multiple worker instances (deferred)
  Rejected because: Not rejected; deferred pending evidence of horizontal scaling requirements. Current evidence shows single cron job context where local rate limiting suffices.
  When valid: Required when workflow monitoring scales horizontally with multiple concurrent worker instances that must share a global rate limit budget to prevent aggregate quota exhaustion.
- Batch workflow initialization operations and process users in fixed-size chunks with inter-batch delays (rejected)
  Rejected because: Fixed batching with delays provides coarse-grained rate control that either wastes quota headroom or risks exceeding limits during variable processing times. Token bucket rate limiting provides finer-grained, adaptive throttling.
  When valid: Suitable for systems with predictable per-user processing times and where simple implementation is prioritized over optimal quota utilization.

## Risks

- Rate limiter configuration drift where tokensPerInterval values become misaligned with external service quota changes, leading to either quota exhaustion or underutilization
  Mitigation: Implement configuration validation that compares rate limiter settings against documented service quotas during deployment. Add monitoring alerts for rate limit approach thresholds.
  Owner: Platform engineering team
- Rate limiting introduces processing delays that cause workflow initialization to exceed acceptable latency bounds, impacting monitoring coverage timeliness
  Mitigation: Establish SLOs for workflow initialization latency and monitor P95/P99 metrics. Tune rate limiter parameters or negotiate quota increases if latency targets are not met.
  Owner: Reliability engineering team
- Version incompatibility between rate limiting library API and locked dependency version causes runtime failures if implementation assumes APIs not present in resolved version
  Mitigation: Enforce LOCK-VERSION GROUNDING policy requiring verification of exact resolved version's API surface before implementation. Include rate limiter instantiation in integration test coverage.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Instantiate rate limiter instances at module scope or as singleton dependencies to ensure consistent rate limiting state across workflow processing iterations within a single process lifecycle.
- Position rate limiting calls immediately before external service invocations in the control flow, after all local data preparation and validation, to maximize quota utilization for successful operations.
- Log rate limiter wait events with user cohort size and elapsed throttle time to enable operational visibility into quota pressure and inform capacity planning decisions.

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and lock artifact, then identify the rate limiting library and its resolved version
- Locate workflow monitoring module source and verify rate limiter instantiation with explicit tokensPerInterval and interval configuration
- Identify the project's test execution mechanism and run integration tests covering workflow initialization with rate limiting enabled

Accept when:
- Rate limiter instantiation is present in workflow monitoring code with explicit tokensPerInterval and interval parameters matching external service quota documentation
- Integration tests demonstrate that bulk workflow initialization operations respect configured rate limits and do not exceed external service quotas
- Code review confirms rate limiting is applied before all external task scheduling service invocations in user enumeration and workflow initialization paths

## Enforcement

- Verified by: Automated code review checks that verify rate limiter instantiation precedes external service client usage in workflow monitoring modules
- Verified by: Integration test suite coverage requirements for rate-limited workflow initialization scenarios
- Verified by: Deployment pipeline validation that confirms rate limiter configuration values align with documented external service quotas
- Violation handling: Pull requests introducing external service calls without rate limiting are blocked by automated review checks and require architectural review approval
- Violation handling: Production incidents caused by quota exhaustion trigger post-incident review to assess rate limiting coverage gaps and update enforcement rules
- Violation handling: Quarterly architecture audits review rate limiter configuration drift against current external service quota documentation
- Exception process: Exception requests must document the specific external service, expected request volume, quota headroom analysis, and monitoring plan
- Exception process: Platform engineering lead reviews exception requests and approves only when quota headroom exceeds expected volume by 3x or service provides separate retry quotas
- Exception process: Approved exceptions are time-bounded with mandatory re-evaluation at next quarterly architecture review