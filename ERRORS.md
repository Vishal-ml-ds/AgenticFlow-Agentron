# Agentron — ERRORS.md

> Self-learning log. Every bug → entry here so the factory never repeats the same mistake.
> Format: `Error → Cause → Fix → Rule → Applies to`

---

## Template

```
### YYYY-MM-DD — <short title>
**Error:** what went wrong (1 line)
**Cause:** root cause (1-2 lines)
**Fix:** the resolution
**Rule:** which Dhruv Rule this maps to (R<n>)
**Applies to:** which module / file pattern / always
```

---

<!-- New entries appended below -->

### 2026-05-14 — Supabase local not started (Docker Desktop unavailable)
**Error:** `npx supabase start` cannot run — Docker daemon is not reachable on this machine (`Docker Desktop is unable to start`).
**Cause:** Docker Desktop on macOS failed to launch. Supabase local stack (Postgres + Auth + Storage + Inbucket) runs as Docker containers; without a running daemon, the local stack cannot come up.
**Fix:** Migrations were authored as canonical `.sql` files in `supabase/migrations/001..003`. They will be applied when (a) Docker is fixed and `supabase start` runs cleanly, OR (b) the cloud Supabase project is provisioned at M10 launch prep. SQL has been linted by reading carefully; the file structure follows the QuotaHit `001_user_credits.sql` style. Cloud provisioning is intentionally deferred per Dhruv's "paid-tier from day 1" rule — billing decision belongs at M10.
**Rule:** R15 (don't assume — verified daemon unreachable, not skipped silently), R14 (test locally then deploy — local dev deferred until Docker available)
**Applies to:** M1 setup, all future Supabase-local tasks until Docker Desktop is repaired.
**Workaround for the rest of M1:** API routes, Zod schemas, types, auth UI, middleware, and Vitest smoke tests are completed and verified via `pnpm typecheck` + `pnpm build`. End-to-end browser test of sign-up → bootstrap → dashboard is BLOCKED on a running Supabase instance and must be re-run by Dhruv (or once Docker is repaired) before the M1 module is declared GREEN.

### 2026-05-14 — Two `supabase start` processes raced in parallel sessions
**Error:** Atlas (M2 backend session) and Angelina (PM session) both tried to bring up the Supabase local stack at the same time. Atlas's foreground `supabase start` returned exit 144; subsequent `supabase status` reported `No such container: supabase_db_agentron`. The two CLIs fought over the same Docker container set.
**Cause:** PM-owns-plumbing (R62) was momentarily violated — Atlas issued `supabase start` directly rather than waiting on Angelina to bring the stack up. The Supabase CLI happily lets you start it from two shells; only Docker arbitrates, and the result is a half-initialised container set neither side can finish.
**Fix:** Stop competing. Engineer sessions (Atlas / Pixel) never run `supabase start | stop | db reset`. PM (Angelina) owns the local stack lifecycle. When the stack is up Angelina pings the engineer to apply migrations + run `\d` verification.
**Rule:** R62 (PM owns plumbing — branches, folders, env vars, dev-server starts, **and the Supabase local stack**)
**Applies to:** every backend module from M2 onwards.

### 2026-05-14 — Inngest function folder deviated from LLD §2 (top-level → apps/web/src/lib/inngest)
**Error:** LLD §2 places Inngest function definitions at top-level `inngest/functions/*`. Placing the M2 `workflow.run.start` function there breaks the Next.js build because (a) the top-level `inngest/` folder is not a pnpm workspace package, so Next does not transpile it, and (b) the function needs `@/lib/inngest/client` and `@/lib/supabase/admin` which only resolve inside `apps/web/src/`.
**Cause:** The LLD was written before the Vercel + Next.js 16 single-app constraint was fully expanded. The top-level folder is a clean conceptual home but not a clean physical home until it becomes a workspace package with its own `package.json` + `tsconfig.json`.
**Fix:** Place the Inngest function at `apps/web/src/lib/inngest/functions/workflow-run-start.ts` and register it via `apps/web/src/app/api/inngest/route.ts`. Document the deviation in CLAUDE.md "Tech Debt" with a trigger to revisit (≥4 Inngest functions OR `inngest/` promoted to a workspace package). Keep `inngest/README.md` at the top-level as the event-→-function map source of truth so the LLD reference still works.
**Rule:** R31 (honest uncertainty — when the LLD path doesn't work, document the deviation, don't silently invent a workaround). R39 (E2E feature completion — the seam works end-to-end, no TODO).
**Applies to:** M2-M9 Inngest functions. Re-evaluate at M10 launch prep.

