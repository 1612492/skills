# Agent guidance and verification

**Priority:** HIGH

## When

Creating or updating the target application's `AGENTS.md`, adding scripts, and validating setup.

## Rule

Before writing `AGENTS.md`, read any existing root/nested agent instructions, architecture docs, package scripts, CI config, and actual folder layout. For a new app, create a concise root `AGENTS.md`; for an existing app, **preserve its binding requirements**, reconcile overlaps, and propose targeted additions instead of replacing it. A copy of this skill is not a substitute for project-specific instructions.

Include, when applicable:

1. Framework/router and actual source paths; ownership of framework routes, feature/page code, shared UI, server-only logic, and query/form schemas. Link to any architecture document rather than duplicating a long snapshot of the tree.
2. Data flow: loader or server prefetch where appropriate, shared TanStack Query options/cache ownership, mutation invalidation, and per-request auth/validation. Describe real app conventions, not hypothetical files.
3. Repo-specific scripts with the **real package manager** and exact script names. A sensible new-project checklist is `test` (non-watch), `typecheck` (or a verified type-check command), `lint`, `format:check`, then `build`. Add a route/SSR smoke check when applicable; run an audit after dependency changes if the project uses one. If a check has not been set up, do not claim it exists.
4. Security guardrails: never expose secrets through public env vars or client imports; enforce authorization and validation inside server endpoints, not only route guards. Never commit real credentials or `.env` files.
5. A reminder to run relevant tests for changed behavior, review the diff and generated files, and report unrun checks and failures. Avoid volatile counts, audit results, version pinning, or one-off status notes as permanent rules.

After edits, install with the project's package manager, run the documented commands, and confirm they work with a clean scaffold or the touched parts of an existing repo. Ensure `AGENTS.md` matches what actually ran; if a check fails, record the failure instead of changing the checklist to make the result look green. Verify formatting and lint ignore generated outputs rather than modifying those outputs. Do not overwrite user changes or broadly format an existing codebase merely to pass bootstrap checks.
