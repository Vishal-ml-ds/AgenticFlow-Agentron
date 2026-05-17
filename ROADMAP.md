# Agentron — Roadmap

> **Build window:** May 14 → Jun 10, 2026 (~4-week MVP sprint)
> **Launch:** Jun 10 (Product Hunt) + Jun 12 (Show HN)
> **Status:** M0 in progress · M1-M10 pending

---

## Milestones

| Module | What | Status | Migration files | API routes | Inngest fns | LOC est |
|--------|------|--------|-----------------|------------|-------------|---------|
| **M0 Repo init** | Monorepo skeleton (apps/web + packages + supabase + inngest + docs) | Shipped (b4949c8) | — | — | — | ~600 |
| **M1 Foundation** | Auth, organizations, workspaces, memberships, RLS | Code-complete (live-DB tests pending — Docker blocked) | 001-003 | `/me`, `/workspaces` (GET, POST), `/workspaces/:id` (GET, PATCH, DELETE), `/workspaces/:id/invite`, `/workspaces/:id/members/:userId` | — | ~1.2k |
| **M2 Workflows core** | Workflows table, runs, steps, chains.json | Pending | 004-006 | `/workflows`, `/runs` | `workflow.run.start` | ~1.8k |
| **M3 Agents skeleton** | LangGraph 5-agent supervisor (Architect, Executor, Observer, Repair, Orchestrator) | Pending | 007 | `/agents` | architect/executor/observer/repair stubs | ~2k |
| **M4 Tools + KB** | MCP connectors, tool catalog, knowledge sources, embeddings | Pending | 008, 008b | `/tools/*`, `/kb/*` | `kb.sync.notion` | ~2.5k |
| **M5 Approvals + audit** | Approvals inbox, audit log, policies, kill-switch | Pending | 009-011 | `/approvals`, `/audit`, `/control-tower/kill-switch` | `approval.requested`, `approval.decided` | ~1.5k |
| **M6 F1 Lead Qualification** (hero) | Sales Desk: leads, scoring rules, enrichment, outreach, proposals | Pending | 012 | `/sales/*` | full F1 workflow live | ~2.5k |
| **M7 Repair Agent** | Reflexion loop, schema drift detection, sandbox replay | Pending | 013 | (consumed internally) | `agent.repair.diagnose`, `agent.repair.patch` | ~3k |
| **M8 Control Tower full** | Real-time metrics, cost ledger, policies, cost-cap | Pending | 014 | `/control-tower/*` | `cost.cap.check` | ~1.5k |
| **M9 Support + Ops** | Tickets, drafts, invoices, F6-F13 workflows | Pending | 015, 016 | `/support/*`, `/ops/*` | F6-F13 workflows | ~3k |
| **M10 UI + Mobile + Launch** | shadcn/ui components, mobile shell (RN + Expo), launch assets | Pending | — | — | — | ~4k UI + 1k mobile |

**Total estimated:** ~25k LOC over ~25 working days.

---

## Definition of Done (per module)

Every module must satisfy ALL of these before the next module starts:

1. Migrations apply cleanly (`supabase db reset` works)
2. RLS verified — cross-workspace query returns 0 rows for non-member
3. All API routes return correct shape for: empty / one tenant / another tenant / unauthorized / malformed
4. `pnpm typecheck` zero errors
5. `pnpm build` zero errors
6. `pnpm test` passes (when tests exist)
7. F12 browser console clean on all module pages
8. Tested with seed data + empty state + cross-tenant isolation
9. `ERRORS.md` updated with any new gotcha
10. `CLAUDE.md` updated if a new convention emerged

No `// TODO` left in committed code (Rule 39). No mock data (Rule 7). No `console.log` (Rule 9).

---

## Current Sprint

**M1 — Foundation** (this commit, D-002)

- [x] Migrations 001 organizations, 002 workspaces, 003 memberships authored
- [x] RLS policies on all three tables, helper `current_user_workspaces()`
- [x] `bootstrap_user` trigger creates default org + workspace + owner membership on signup (uses `set row_security = off` + exception handler per IW-006)
- [x] Hand-written Supabase types in `packages/shared/src/database.types.ts` (regenerate when local stack returns)
- [x] Zod schemas in `packages/shared/src/zod/workspaces.ts` (every UI-rendered field declared — R43)
- [x] 5 API routes (8 endpoints): `/me`, `/workspaces` (GET, POST), `/workspaces/[id]` (GET, PATCH, DELETE), `/workspaces/[id]/invite` (POST), `/workspaces/[id]/members/[userId]` (DELETE)
- [x] Standard `ApiResponse<T>` envelope on every endpoint with `request_id` + `latency_ms`
- [x] Auth UI scaffolds: `/sign-in`, `/sign-up`, `/callback`
- [x] Protected `/dashboard` redirects unauthenticated users (middleware-driven)
- [x] Vitest config + 8 smoke tests — all pass
- [x] `pnpm typecheck` zero errors
- [x] `pnpm build` zero errors
- [x] `pnpm test` 8/8 pass
- [ ] Live Supabase verification (sign-up flow E2E, RLS cross-tenant check, `\d` table inspection) — BLOCKED until Docker Desktop is repaired OR cloud Supabase project is provisioned (M10). Documented in `ERRORS.md` 2026-05-14.

**M0 — Repo Init** (shipped Dhruv-verified commit `b4949c8`)

- [x] Clone repo
- [x] Create develop branch
- [x] Monorepo skeleton
- [x] Stack pinned (Next.js 16, React 19, TS 5, Tailwind 4, Inngest 3.54, LangGraph, Supabase, Zod)
- [x] `pnpm install`, `pnpm typecheck`, `pnpm build` — all green
- [x] Push to `origin/develop`

Next: D-003 M2 Workflows core (workflows, runs, steps, chains.json). Do NOT start until M1 is verified by Dhruv against a live Supabase instance.
