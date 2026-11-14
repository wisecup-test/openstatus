---
version: 2
name: ef7d3651-fc9e-4deb-b4d3-7f18ab60fda1-adoption-of-openstatus-db-for-data-access-and-schema-definition-persistent-data-access-query-execution-schema
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Persistent Data Access Query Execution Schema"
metadata:
  actual:
    adr_id: ef7d3651-fc9e-4deb-b4d3-7f18ab60fda1
    status: accepted
    candidate_group_id: da7123d9-0b29-4b39-b7d6-dfa708206c4f
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
---

# Adoption of @openstatus/db for Data Access and Schema Definition: Persistent Data Access Query Execution Schema

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/checker/**`
- `src/cron/**`
- `src/incident/**`
- `src/lib/**`
- `src/scripts/**`

## Context

- Background workflows, scheduled tasks, and incident checkers require persistent access to domain models including workspaces, monitors, notifications, and incident records.
- Dispersed background services and task handlers risk schema divergence and duplicated client configuration when persistence logic is decentralized.
- The codebase centralizes relational schema definitions and database connection management into a shared internal workspace package.

## Problem Statement

Background execution jobs and cron workers across the workflows subsystem require consistent, strongly typed access to core entities without duplicating database schemas or connection lifecycles across multiple discrete handlers.

## Decision

1. MUST: All persistent data access, query execution, and schema references across background workflows and task runners MUST route through the @openstatus/db shared package.

## Policy Block

- MUST All persistent data access, query execution, and schema references across background workflows and task runners MUST route through the @openstatus/db shared package.

In scope:
- Background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

Out of scope:
- Stateless in-memory computations, isolated client-side interfaces, and third-party external service calls that do not interact with the primary application database.

## Rationale

- Evidence across 13 workflow and task modules demonstrates universal adoption of @openstatus/db for query building and schema access.
- Centralizing database interaction prevents entity definition drift and ensures transactional and query consistency across scheduled and event-driven workers.
- Standardizing on the shared database package simplifies connection pooling and database driver configuration across the distributed workflow runtime.

## Consequences

Positive:
- Provides a single source of truth for relational schemas and entity definitions across all background routines.
- Guarantees consistent query construction and compile-time type safety across scheduled jobs and event handlers.
- Centralizes connection pooling, driver configuration, and lifecycle management for the persistent store.

Negative:
- Workflow routines become tightly coupled to the schema evolution and deployment cadence of the shared package.
- Changes to core schema definitions necessitate coordinated verification across all consuming task modules.

## Alternatives

- Decentralized Database Access with Local Schema Declarations (rejected)
  Rejected because: Declaring database schemas and connection instances within individual workflow modules creates schema drift, duplicate maintenance overhead, and uncoordinated connection pooling.
  When valid: Valid only in strictly isolated microservices with distinct datastores and completely disjoint entity models.
- Direct Raw SQL Execution in Each Task Handler (rejected)
  Rejected because: Bypassing the shared package and schema layer removes compile-time type guarantees and increases the likelihood of unhandled query syntax errors.
  When valid: Valid for ad-hoc database migration scripts or low-level performance diagnostics where schema abstraction imposes unacceptable latency.

## Risks

- Breaking schema changes in the shared package can simultaneously impact multiple background jobs and cron routines.
  Mitigation: Implement non-breaking schema evolution strategies, deprecation cycles, and automated regression testing across all dependent workflow jobs.
  Owner: engineering team
- Uncontrolled connection creation across diverse scheduled tasks could exhaust database connection pools.
  Mitigation: Manage connection limits and client instantiation through centralized configuration within the database package.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import query builder instances and entity schemas directly from the shared database package rather than initializing distinct database connections.
- Apply schema validation guards prior to persisting mutated status or incident records to ensure database constraints are respected.

## Continuation Context


Verify commands:
- Discover the repository typecheck script from the project manifest and execute it to ensure all database queries and schema references satisfy static types.
- Discover the repository test runner from the project manifest and execute the test suite covering background workflow jobs and database operations.

Accept when:
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

## Enforcement

- Verified by: Automated continuous integration checks executing static type inspection and project test suites.
- Verified by: Peer code review verifying that all persistent data interactions import from the designated shared package.
- Violation handling: Pull requests introducing direct database drivers or duplicate schema declarations outside the shared package will be blocked from merging.
- Exception process: Architectural review and formal sign-off from the platform engineering team are required prior to introducing alternative database clients or independent datasources.