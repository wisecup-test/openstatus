<!-- actual-ai:adr-governance:start -->
# Project ADRs

This project's conventions are encoded as ADRs under `.actual/rules/`. **The ADRs ARE the pattern.** Follow them verbatim instead of reading existing implementations to figure out how to do something.

> **Note:** this directive is calibrated for one-shot tasks (a single discrete feature). For multi-task interactive sessions, consult the ADRs for each task transition rather than holding to the per-session caps below.

## Workflow (follow in order)

1. **Identify topic.** Match the files you'll edit against the path-glob table below. Pick the 1-3 topics that match. Do not pre-emptively pick "related" topics; pick only what the file paths actually match.

2. **Select ADRs by filename — the filename is the index.** Run `ls .actual/rules/`. Each filename is `<topic>-<aspect-slug>-<hash>.md`; the `<aspect-slug>` (middle segment) names the ADR's specific concern — e.g., `database-schema-defined`, `zod-input-validation`, `cache-key-format`.

   **Scan ALL filenames first, then pick only the ones whose aspect-slug directly names a noun or verb in your task.** Select by filename; never read a body to decide relevance. **Hard cap: read at most 5 ADR files total.** If more than 5 look relevant, you are over-matching — keep the 5 most specific.

   **If the path-glob table below is a single `**/*` → `cross-cutting-` row** (one big bucket, no per-area topics), this filename scan is your ONLY filter. Do **not** read the bucket exhaustively — treat the filenames as a menu, match aspect-slugs to your task, read ≤5, and ignore the rest. Reading every ADR in the bucket is the exact failure this directive exists to prevent.

3. **Locate insertion points (one read per file, max 3 files).** You may read source files ONLY to (a) find where to add code (which directory, which barrel export to update) or (b) look up an exact identifier you must import. **Do not read source files as pattern examples — the ADRs already encode the pattern.** If you find yourself reading a file because "I want to see how X is done elsewhere," stop. The ADR you already read tells you how.

4. **Implement.** Write the code following the rule statements verbatim. If two ADRs seem to conflict, follow the more specific one (longer topic prefix wins).

5. **Verify after implementing.** Only after the code is written, re-read the `verify_commands` or `accept_criteria` sections of the ADRs you applied and check your work against them. Run the verify commands if any.

## Anti-patterns to avoid

- Reading the first N rules alphabetically because they're cheap. Filter by aspect-slug first, then read only the relevant ones.
- Reading >5 ADR files for a single feature. If you're tempted, you're over-scoping the topic match.
- Reading the entire `cross-cutting-` bucket because "every rule is always active." Selection is by filename (step 2); you apply the ≤5 you selected, not all of them.
- Reading existing similar features to "see the pattern" — the ADRs encode the pattern. Trust them.
- Re-reading the same ADR multiple times. Cache it mentally.
- Continuing to browse the codebase after step 3. By step 4 you should be writing, not reading.

Each rule file at `.actual/rules/<topic>-<aspect>-<hash>.md` contains the full ADR with rule statements, verify commands, and accept criteria.

## Verification Protocol

These rules are ALWAYS ACTIVE. Apply every rule **from the ADRs you selected in step 2** that governs the files you touch — to all code generation, modification, and review. "Always active" does **not** mean read every ADR: you apply the handful you selected by filename, within the read cap above.

Every rule follows a **Verify → Fix → Repeat** loop. After generating or modifying code for any rule you MUST:

1. **RUN** the rule's `### Verify` command(s).
2. **CAPTURE** the full output (stdout + stderr).
3. **EVALUATE** the output against the rule's **Accept when** criteria.
4. **IF FAILING:** diagnose the root cause, apply a fix, and re-run from step 1.
5. **IF PASSING:** keep the passing output as evidence before moving on.
6. **MAX ITERATIONS:** 5 attempts per rule. If still failing after 5 attempts, STOP and report the failure with all captured output.

Compliance is not optional. Do not skip verification, assume correctness, or defer it to a later task. Every change to a governed area must be accompanied by a passing verification run.

## Mandatory Dependency Grounding

Before writing or modifying implementation code that uses an external dependency:

1. Identify every affected external dependency.
2. Locate the repository's manifest and lock or resolution artifact.
3. Determine the exact repository-resolved version from the lock artifact.
4. Verify that the active environment matches that version.
5. Verify each API being introduced or changed against evidence applicable to that exact version (official documentation or public API reference).
6. Produce a dependency-grounding record.
7. Stop if any version, environment, or API cannot be verified.

Do not begin implementation until dependency grounding has passed.

The repository lock or resolution artifact is authoritative. A manifest range or model recollection is not sufficient.

### Integrated workflow

1. Stabilize the workspace.
2. Inspect the task and relevant architecture decisions.
3. Identify affected dependencies.
4. Run dependency grounding.
5. Produce the implementation plan.
6. Implement the change.
7. Validate grounding coverage, build, types, lint, and tests.
8. Report evidence, assumptions, and blockers.

## Path glob → topic

| You're editing | Topic prefix |
|---|---|
| `**/*` | `cross-cutting-` _(445 ADRs)_ |
<!-- actual-ai:adr-governance:end -->

# Agent.md

This file provides a comprehensive overview of the OpenStatus project, its architecture, and development conventions to be used as instructional context for future interactions.

## Project Overview

OpenStatus is an open-source synthetic monitoring platform. It allows users to monitor their websites and APIs from multiple locations and receive notifications when they are down or slow.

The project is a monorepo managed with pnpm workspaces and Turborepo. It consists of several applications and packages that work together to provide a complete monitoring solution.

### Core Technologies

-   **Frontend:**
    -   Next.js (with Turbopack)
    -   React
    -   Tailwind CSS
    -   shadcn/ui
    -   tRPC
-   **Backend:**
    -   Hono (Node.js framework)
    -   Go
-   **Database:**
    -   Turso (libSQL)
    -   Drizzle ORM
-   **Data Analytics:**
    -   Tinybird
-   **Authentication:**
    -   NextAuth.js
-   **Build System:**
    -   Turborepo

### Architecture

The OpenStatus platform is composed of three main applications:

-   **`apps/dashboard`**: A Next.js application that provides the main user interface for managing monitors, viewing status pages, and configuring notifications.
-   **`apps/server`**: A Hono-based backend server that provides the API for the dashboard application.
-   **`apps/checker`**: A Go application responsible for performing the actual monitoring checks from different locations.

These applications are supported by a collection of shared packages in the `packages/` directory, which provide common functionality such as database access, UI components, and utility functions.

## Building and Running

The project can be run using Docker (recommended) or a manual setup.

### With Docker

1.  Copy the example environment file:
    ```sh
    cp .env.docker.example .env.docker
    ```
2.  Start all services:
    ```sh
    docker compose up -d
    ```
3.  Access the applications:
    -   Dashboard: `http://localhost:3002`
    -   Status Pages: `http://localhost:3003`

### Manual Setup

1.  Install dependencies:
    ```sh
    pnpm install
    ```
2.  Initialize the development environment:
    ```sh
    pnpm dx
    ```
3.  Run a specific application:
    ```sh
    pnpm dev:dashboard
    pnpm dev:status-page
    pnpm dev:web
    ```

### Running Tests

To run the test suite, use the following command:

Before running the test you should launch turso dev in a separate terminal:
```sh
turso dev
```

Then, seed the database with test data:

```sh
cd packages/db
pnpm migrate 
pnpm seed
```

Then run the tests with:

```sh
pnpm test
```

## Development Conventions

-   **Monorepo:** The project is organized as a monorepo using pnpm workspaces. All applications and packages are located in the `apps/` and `packages/` directories, respectively.
-   **Build System:** Turborepo is used to manage the build process. The `turbo.json` file defines the build pipeline and dependencies between tasks.
-   **Linting and Formatting:** The project uses oxlint for linting and oxfmt for formatting. The configuration can be found in `oxlint.config.ts` and `oxfmt.config.ts`.
-   **Code Generation:** The project uses `drizzle-kit` for database schema migrations.
-   **API:** The backend API is built using Hono and tRPC. The API is documented using OpenAPI.

## Comment Discipline

Default to writing no comments. The code and identifiers should explain *what* — the reader can see that. Only write a comment when the *why* would not be obvious from reading the code: a non-obvious invariant, a workaround for a specific bug, a runtime guarantee that justifies a cast, a constraint imposed from outside this file.

-   **Keep them to 1 short line where possible**, 3 lines max. Never write multi-paragraph JSDoc blocks.
-   **Strip these every time:** restating what the code does, naming the caller / surface that uses the helper, history ("added for X", "used by the Y flow"), and PR/task context. That belongs in commit messages, not source.
-   **Keep these:** the WHY behind a non-obvious choice, an invariant that callers must uphold, a `// safe because …` line above an unavoidable cast, a `// workaround: <bug>` for a known issue.
-   **JSDoc:** allowed on exported symbols when the type signature alone is ambiguous — but one sentence, not a tutorial. Don't enumerate every branch of a function in prose.

If you find yourself writing a comment that explains *what just changed* or *what you did*, delete it.

## Type Cast Discipline

`as unknown as X`, `as never`, and `as any` are sometimes unavoidable — usually at boundaries with external SDKs (AI SDK, third-party libs) or at registry-style dispatch where TypeScript can't link a runtime string to a literal-keyed map. When you need one:

-   **Centralize the cast in a named helper.** Don't scatter the same cast across call sites. Wrap it in a small function whose name describes the intent (`asUIMessages`, `findRenderer`, `renderToolDraft`).
-   **Comment the runtime guarantee.** Above the helper, write one or two lines explaining *why the cast is safe at runtime* (e.g. "the persisted shape is validated on write by `storedMessageSchema`, so reads return SDK-conforming rows"). Future readers can verify the invariant or notice when it breaks.
-   **Examples:** `apps/dashboard/src/components/chat/use-chat-session.ts` (`asUIMessages`); `apps/dashboard/src/components/chat/tool-renderers/index.tsx` (`renderToolDraft` / `renderToolResult` / `summarizeToolOutput`). Both eliminate scattered casts in the consuming components.

A scattered `as never` is usually a missing helper.

## Services & Audit Log Pattern

All workspace-scoped business logic lives in `packages/services` — **not** in tRPC routers. Routers stay thin: validate input, call a service verb, map errors. This keeps logic reusable across tRPC, Hono, and background jobs.

Note on runtimes: the dashboard's tRPC handler runs on the **Node.js** runtime (see the comment in `apps/dashboard/src/app/api/trpc/lambda/[trpc]/route.ts` — several routers pull Node-only deps), and no Edge route imports `@openstatus/services`. Node-only dependencies in services are therefore acceptable. What *is* enforced: `apps/workflows` runs on Deno and imports services, so `pnpm check` (`deno check --sloppy-imports`) must pass in both packages.

Conventions for any new mutation:

-   **One file per verb** under `packages/services/src/<entity>/` (e.g. `create.ts`, `update.ts`, `remove.ts`), re-exported from the entity's `index.ts`. Routers import from `@openstatus/services/<entity>`.
-   **Standard signature:** `async function verbEntity(args: { ctx: ServiceContext; input: VerbInput }): Promise<...>`. `ctx` carries `workspace`, `actor`, and an optional `db`/transaction. Parse input with the schema at the top of the function.
-   **Wrap mutations in `withTransaction(ctx, async (tx) => { ... })`** — it reuses an outer tx if present, otherwise opens one. Always pass `tx` (not `defaultDb`) to writes inside the block.
-   **Workspace scoping is mandatory.** Every read/write filters by `ctx.workspace.id`. Use the `getXInWorkspace` helpers in `internal.ts` for fetch-or-throw.
-   **Throw `ServiceError` subclasses** (`NotFoundError`, `ForbiddenError`, etc. from `./errors`). Routers convert them via `toTRPCError`.
-   **Emit an audit row for every mutation** via `emitAudit(tx, ctx, entry)` inside the same transaction. Fail-closed: a failed audit insert rolls back the mutation. See `packages/services/src/audit/emit.ts`.
    -   For updates, pass both `before` (pre-mutation snapshot) and `after` (post-`.returning()` row). `changed_fields` is auto-diffed; no-op updates are skipped.
    -   For creates/deletes, pass only `after` or only `before`.
    -   **Strip secrets** from snapshots before emitting (e.g. `credential`, bot tokens, raw API keys) — see `integration/remove.ts` for the pattern.
    -   Action names follow `{entity}.{verb}` (`monitor.update`, `integration.delete`). Add new variants to the discriminated union in `@openstatus/db/src/schema/audit_logs/validation.ts`.
-   **Tests live in `packages/services/src/<entity>/__tests__/`** and use `expectAuditRow({ workspaceId, action, entityId, ... })` from `packages/services/test/helpers.ts` to assert the audit side-effect. Each suite scopes to its own workspace and clears `audit_log` between cases.

When adding a router endpoint, the default answer is "write the service verb first, then call it from the router." Inline DB access in routers is a smell — it bypasses the audit log.

## Scope Enforcement (API key RBAC)

API keys carry **scopes** (`'read'` / `'write'`) that gate write access. See `packages/services/src/auth/`.

-   **Call `requireScope(ctx, "write")` as the first line** of every write verb — before `Input.parse(...)` and `withTransaction`. Import from `../auth`.
-   **"Write"** = any DB mutation **or** side-effecting external call (probes, webhooks, notifications). Everything else is read.
-   No-op for `user` / `system` / `slack` / `webhook` / `subscriber` actors; active for `apiKey` and `mcp`. Throws `ForbiddenError` on denial.
-   **Tests:** every entity's `__tests__/` includes a `'rejects read-only actor'` case via `makeApiKeyCtx(workspace, { keyId: "k", userId: 1, scopes: ["read"] })`. `requireScope` fires before DB lookup, so fake ids work for delete/update verbs.
-   **MCP tools** declare `scope: 'read' | 'write'` and register via `registerScopedTool` — read-only keys never see write tools.

Treat a missing `requireScope` the same as a missing `emitAudit` — mandatory for every mutation.
