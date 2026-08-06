# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Usage Metadata

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system requires secure generation of API keys for authentication purposes, where keys must be unpredictable and resistant to brute-force attacks
- API keys serve as bearer tokens that grant access to protected resources, necessitating cryptographically secure random generation to prevent enumeration attacks
- Stored API key hashes must be protected against rainbow table and precomputation attacks through adaptive hashing with computational cost factors
- The authentication module exposes public contracts for key generation, hashing, verification, and usage tracking, indicating a centralized credential management pattern

## Problem Statement

Authentication systems require a secure mechanism to generate, store, and verify API keys that balances security requirements with performance constraints while preventing common attack vectors such as brute-force enumeration, rainbow table lookups, and timing attacks.

## Decision

1. SHOULD: API key usage metadata SHOULD be tracked to support security auditing and anomaly detection

## Policy Block

- SHOULD API key usage metadata SHOULD be tracked to support security auditing and anomaly detection

In scope:
- API key generation for authentication purposes
- API key storage and persistence
- API key verification during authentication flows
- Credential management utilities within the authentication module

Out of scope:
- Session tokens or JWT generation
- OAuth or SAML authentication flows
- Password hashing for user accounts
- Encryption of data at rest or in transit beyond credential storage

## Rationale

- The evidence shows explicit use of cryptographic random generation and adaptive hashing libraries, indicating a deliberate security-first approach to credential management
- The pattern of exposing separate public contracts for generation, hashing, and verification demonstrates architectural separation between key lifecycle stages
- Adaptive hashing with configurable work factors provides future-proof protection as computational capabilities increase
- The detection of usage tracking functions suggests the system implements security monitoring beyond basic authentication

## Consequences

Positive:
- Cryptographically secure random generation prevents enumeration and prediction attacks on API keys
- Adaptive hashing with work factors provides resistance to brute-force attacks that scales with attacker computational resources
- Constant-time comparison operations eliminate timing side-channels during verification
- Modular public contracts enable consistent credential handling across the application

Negative:
- Adaptive hashing introduces computational overhead during key creation and verification, potentially impacting authentication latency
- Work factor configuration requires careful tuning to balance security and performance for the deployment environment
- Dependency on external cryptographic libraries introduces supply chain risk and requires ongoing security updates
- Increased complexity in credential management compared to simpler but less secure approaches

## Alternatives

- Use standard random number generation without cryptographic guarantees (rejected)
  Rejected because: Non-cryptographic random generation produces predictable sequences that enable enumeration attacks, violating fundamental security requirements for bearer tokens
  When valid: Never valid for security-sensitive credential generation
- Store API keys in plaintext or using reversible encryption (rejected)
  Rejected because: Plaintext or reversible storage exposes all credentials if the database is compromised, whereas one-way hashing limits exposure to individual key verification
  When valid: Never valid for production authentication systems
- Use fast cryptographic hashes without adaptive work factors (rejected)
  Rejected because: Fast hashes enable efficient brute-force attacks using modern GPU hardware, while adaptive hashing with work factors maintains security as computational power increases
  When valid: Only acceptable for non-security-critical checksums or integrity verification

## Risks

- Insufficient entropy in random generation could reduce key space and enable prediction attacks
  Mitigation: Use platform-provided cryptographically secure random sources with documented entropy guarantees and validate minimum key length requirements
  Owner: Security Engineering Team
- Work factor misconfiguration could result in either inadequate security or unacceptable performance degradation
  Mitigation: Establish baseline work factor through performance testing in production-like environments and implement monitoring for authentication latency
  Owner: Engineering Team
- Vulnerabilities in cryptographic library dependencies could compromise all stored credentials
  Mitigation: Implement automated dependency scanning, subscribe to security advisories for cryptographic libraries, and maintain update procedures for rapid patching
  Owner: Security Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Establish work factor baselines through load testing that measures authentication latency under expected concurrent request volumes, targeting sub-second response times while maximizing computational cost
- Implement key rotation mechanisms that allow graceful migration from old to new keys without service disruption, supporting both active and deprecated keys during transition periods
- Consider implementing rate limiting and anomaly detection on API key usage patterns to detect compromised credentials before significant damage occurs

## Continuation Context


Verify commands:
- Locate the project's test suite directory structure and identify verification scripts for authentication utilities, then execute the discovered test runner with appropriate filters for credential management tests
- Inspect the project's continuous integration configuration to identify security scanning and dependency audit commands, then execute those commands to verify cryptographic library versions and known vulnerabilities
- Search the repository for static analysis or linting configurations that enforce secure coding patterns, then run the discovered analysis tools to verify compliance with cryptographic best practices

Accept when:
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources

## Enforcement

- Verified by: Automated test suites covering key generation, hashing, and verification operations
- Verified by: Static analysis tools detecting insecure random generation or plaintext credential storage
- Verified by: Dependency scanning in continuous integration pipelines
- Verified by: Security-focused code review for authentication module changes
- Violation handling: Build failures block merge for test failures or static analysis violations
- Violation handling: Security scan findings trigger immediate review and remediation workflow
- Violation handling: Code review rejections require revision before approval
- Violation handling: Runtime monitoring alerts on authentication anomalies trigger incident response
- Exception process: Exception requests must document specific technical constraints preventing compliance
- Exception process: Security engineering team reviews and approves all exceptions with compensating controls
- Exception process: Approved exceptions are time-bound and require periodic re-evaluation
- Exception process: Exception rationale and compensating controls are documented in code comments and architecture documentation