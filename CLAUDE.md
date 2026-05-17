# Agentron — AI Business Operations OS

> **Tagline:** Workflows that work themselves.
> **Owner:** Dhruv Tomar
> **Course:** Euron · AI Product Engineering 2.0 (Project 2 of 10)
> **Build window:** May 14 → Jun 10, 2026 · **Launch:** Jun 10 PH + Jun 12 Show HN
> **Status:** M0 Repo Init shipped. M1 Foundation next.

---

## What Agentron Is

**AI Business Operations OS** — three product lines (Sales Desk + Support Desk + Ops Desk) on a generic workflow builder. Operate workflows once via NL prompt or drag-drop canvas; agents architect, execute, observe, repair.

**Wedge ICP:** "Priya the Agency Operator" — 10–50 person AI/automation agency running 20–80 client workflows.

**Differentiator:** AI agents that build, run, and self-heal workflows. Multi-tenant, open-core (AGPL + Commercial), web + mobile day 1.

---

## The 4 Active Docs (read in order)

| # | Doc | Owner | Status |
|---|-----|-------|--------|
| 01 | `docs/01-BRD.md` — Business Requirements | Dhruv | Locked |
| 02 | `docs/02-PRD.md` — 20-section PRD, 16 features (F1-F16) | Dhruv | Locked |
| 03 | `docs/03-ARCHITECTURE.md` — System design + 5-agent topology | Dhruv | Locked |
| 04 | `docs/04-LLD.md` — Schema, API map, module structure | Dhruv | Locked 2026-05-12 |

**Then:** code in `apps/web/src/`, `packages/`, `supabase/migrations/`, `inngest/functions/`.

**Source hierarchy:** 02-PRD > 01-BRD > 03-ARCHITECTURE > 04-LLD > anything in `docs/_archive/`.

---

## Locked Decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-05-03 | Lead: Dhruv (PRD #2 + Architecture #5 + Repo #3 + Docs #11) | Sudhanshu's 14-step tracker |
| 2026-05-03 | Mobile (Android + iOS) IN SCOPE day 1 | Stack: React Native + Expo + EAS (deferred to M10) |
| 2026-05-05 | Feature scope: 16 features (Sales 5 + Support 5 + Ops 3 + Platform 3) | Team-edited Google Doc |
| 2026-05-05 | License = AGPL v3 + Commercial Exception (open-core) | PRD §18 |
| 2026-05-05 | Pricing: Free / Pro $49 / Team $99 / Business $299 / Lifetime $299 first 100 | PRD §18 |
| 2026-05-12 | Stack locked: **TypeScript full-stack on Vercel + Supabase + Inngest** (NOT FastAPI) | One deploy, ship today |
| 2026-05-14 | Brand renamed: EuriFlow → **Agentron** | This repo |
| 2026-05-14 | **Public repo = docs-only, private repo = code.** `origin` = `aiagentwithdhruv/agentron` (PRIVATE). `public-docs` remote = `euron-sudh/PROJECT-2-EuriFlow-...` (docs/governance only, force-pushed orphan root). Code merges to public as `v1.0.0` at launch. Local backup at `agentron-build-backup-2026-05-14/`. | Public face shows clean blueprint to Sudhanshu/reviewers; team ships in working repo without WIP noise. |

---

## Tech Stack — LOCKED in 04-LLD.md §1

| Layer | Choice |
|-------|--------|
| Monorepo | Turborepo + pnpm workspaces |
| Web | Next.js 16 + React 19 + TypeScript 5 + Tailwind 4 + Radix UI |
| Backend | Next.js 16 API routes (no separate FastAPI) |
| Workflow runtime | Inngest 3.54+ (durable, retry-safe) |
| Agent framework | `@langchain/langgraph` (TypeScript) — supervisor pattern |
| Tool layer | `@modelcontextprotocol/sdk` (TS) + per-tool REST clients |
| DB | Supabase (Postgres 15 + pgvector + Auth + RLS) — **paid tier from day 1** |
| Cache + queue | Upstash Redis (serverless, Vercel-native) |
| Storage | Supabase Storage (S3-compatible) |
| Email | Resend |
| Payments | Stripe |
| LLM | Euri Gateway → OpenRouter → Anthropic / OpenAI direct |
| Deploy | Vercel (auto from `main`) |
| Observability | Sentry + Langfuse + Vercel Analytics |
| CI/CD | GitHub Actions: lint + typecheck + build on every PR |

---

## Dhruv's 65 Rules (always-on)

This project follows `../Angelina-OS/rules/DHRUV-RULES.md` exactly.

Critical for builds:

| Phase | Rule heading |
|-------|--------------|
| 0 — System Design | R1 Map full system · R2 PRD→HLD→LLD→CLAUDE→Build · R3 Design screens before code |
| 1 — Spec | R4 Spec per feature · R5 Feature branch per feature |
| 2 — Build | R6 Backend first (schema=contract) · R7 NO MOCK DATA · R8 One module at a time |
| 3 — Quality | R9 Test like a user (10-pt checklist) · R10 Validate against spec before merge · R20 Build check before done |
| 4 — Architecture | R11 Don't design for scale day 0 · R12 Connection-ready FKs · R13 Security non-negotiable |
| 5 — Deploy | R14 Local→Tunnel→Staging→Prod, never skip · R38 Work on develop, never push to main |
| 6 — Knowledge | R15 Don't assume verify · R16 Factory learns from mistakes · R17 Don't use AI when rules work · R39 No TODOs, E2E or BLOCKED |

---

## Repo Layout

```
agentron-build/
├── CLAUDE.md                      ← This file (master context)
├── README.md                      ← Public one-pager
├── ROADMAP.md                     ← Milestone tracker (M0-M10)
├── ERRORS.md                      ← Self-learning log
├── package.json                   ← Workspace root
├── pnpm-workspace.yaml
├── turbo.json
├── vercel.json
├── tsconfig.json
├── tsconfig.base.json
├── .env.example
├── .gitignore
├── apps/
│   └── web/                       ← Next.js 16 app (landing + auth + dashboard + API routes)
│       ├── src/app/
│       │   ├── (auth)/
│       │   ├── (app)/             ← dashboard, workflows, runs, approvals, tools, control-tower
│       │   └── api/               ← all backend routes under /api/v1
│       ├── src/lib/
│       │   ├── supabase/          ← client, server, middleware
│       │   ├── inngest/           ← Inngest client
│       │   ├── llm/               ← Euri Gateway adapter
│       │   └── utils.ts
│       └── middleware.ts          ← Supabase auth
├── packages/
│   ├── agents/                    ← LangGraph 5-agent supervisor (filled in M3)
│   ├── tools/                     ← MCP connectors (filled in M4)
│   ├── workflows/                 ← Workflow templates (filled in M6+)
│   └── shared/                    ← Types, Zod schemas, constants (used everywhere)
├── supabase/
│   ├── migrations/                ← SQL 001-016 (M1 starts)
│   ├── seed.sql
│   └── config.toml
├── inngest/
│   └── functions/                 ← Inngest function definitions (M2+ fills)
└── docs/
    ├── 01-BRD.md
    ├── 02-PRD.md
    ├── 03-ARCHITECTURE.md
    └── 04-LLD.md
```

---

## Coordination Model

**Dispatch flow** (Angelina-OS pattern):

1. Lead writes spec → posts to `EuriFlow/dispatches/D-NNN-slug.md`
2. Lead pastes prompt into team member's fresh Claude Code session (no auto-spawn — hard rule R61)
3. Team member commits to `develop` branch (never `main`)
4. Lead reviews PR + merges develop → main
5. Lead starts dev server + verifies in browser before declaring done

**Comms:** WhatsApp · Weekly sync Sun 7-11 PM IST · All decisions logged in this CLAUDE.md table

---

## What NOT to Do

- Don't start the next module before the current one passes the Definition of Done in `ROADMAP.md`
- Don't use free-tier Supabase (paid from day 1 — past burn at IW)
- Don't auto-deploy collaborator pushes (PR review mandatory)
- Don't spawn coding sub-agents from leads' sessions (R61 — write prompt → paste manually)
- Don't dispatch repo/folder/branch creation to team members (R62 — PM owns plumbing)
- Don't ship `// TODO` in committed code (R39 — feature is E2E complete or BLOCKED)
- Don't ship mock data (R7 — empty API → "No data yet")
- Don't reference anything in `docs/_archive/` as authoritative

---

## Tech Debt (Tracked)

| Item | Why | Resolved at |
|------|-----|-------------|
| Cloud Supabase project provisioning deferred to M10 launch prep — see `ERRORS.md` 2026-05-14 entry | "Paid-tier from day 1" rule + billing decision belongs at launch prep. Migrations applied via `supabase db reset` against the cloud project at M10. | M10 |
| Supabase local stack not yet runnable on this machine (Docker Desktop fails to start) — see `ERRORS.md` | Without local Postgres, `pnpm supabase start` + `supabase db reset` cannot be exercised. SQL migrations are authored + lint-reviewed but the live `\d` table inspection + RLS round-trip test are pending. | When Docker Desktop is repaired, OR when cloud project is provisioned at M10 |
| Supabase TypeScript types are hand-written in `packages/shared/src/database.types.ts` (vs. `supabase gen types --local`) | Generator requires a running local stack; types mirror the migration files manually. Regenerate with `npx supabase gen types typescript --local > packages/shared/src/database.types.ts` once Supabase local is available. M6 extended the file with leads / scoring_rules / outreach_sequences and added `pending_chains_step_id` on workflow_runs. | When Supabase local boots cleanly |
| `agentron` internal tool — credentials are a `{ workspace_id }` jsonb, not a real secret. The connector uses the service-role admin client directly because it has no external API to call. | The internal connector handles ICP scoring + lead-row mutations from inside workflow steps. M10 may swap to a single in-process step kind (`kind=internal`) so the tool layer doesn't need a placeholder catalog entry. | M10 |
| M7 Sandbox replay runs against real workspace credentials — full isolated execution environment is deferred to M7.1 | Sandbox runs use `is_sandbox=true` + `parent_run_id` to keep them distinguishable, and the coordinator refuses to sandbox-replay any graph that touches a tool without dry-run support (currently anything other than `agentron` / `perplexity` / `notion`). This is acceptable for Free/Pro tiers but is a launch-blocker for Business/Enterprise unsupervised self-heal. | M7.1 (post-launch) |
| Inngest functions live at `apps/web/src/lib/inngest/functions/` instead of top-level `inngest/functions/` per LLD §2 | The top-level `inngest/` folder is not a pnpm workspace package, so Next.js does not transpile it and the function couldn't `import { inngest } from "@/lib/inngest/client"` without fragile relative paths. Co-locating with the rest of the Next app keeps the build simple at M2 scale. The top-level `inngest/` folder is retained as a placeholder (`.gitkeep` + README) and can be promoted to a workspace package when M3+ adds enough Inngest functions to justify it. | When ≥4 Inngest functions exist OR top-level `inngest/` becomes a workspace package |

---

## Convention: Multi-Tenant FKs (R12 — Connection-Ready)

Every tenant-scoped table in this codebase MUST FK to `workspaces(id)` on a column named `workspace_id`. This is the *only* tenant column. RLS policies on every such table use the helper `public.current_user_workspaces()` defined in `supabase/migrations/003_memberships.sql`:

```sql
alter table <new_table> enable row level security;
create policy "tenant_isolation" on <new_table>
  using (workspace_id in (select * from public.current_user_workspaces()));
```

No exceptions. Tenant column is `workspace_id`, helper is `current_user_workspaces()`, every new migration follows this template.

---

## References

- **Production domain:** [agentron.site](https://agentron.site) (Namecheap, May 5 2026 → May 5 2027, BasicDNS, redirects to www currently)
- Master Allocation Sheet: https://docs.google.com/spreadsheets/d/1lBFUPR-p2jHxcomAlp5SACEHBlAzrFOE/edit
- **Canonical PRD (Google Doc):** https://docs.google.com/document/d/1maSp68WLOrrKRivCqnqO6xhP9OCgasaU6sgTNO4IwQg/edit (Agentron_PRD_v1_1, 31 sections)
- Course: https://euron.one/course/ai-product-engineering-2-0
- Dhruv Rules: `../Angelina-OS/rules/DHRUV-RULES.md`
- LLD: `docs/04-LLD.md` — the blueprint
- Pixel chain: `../EuriFlow/prompts/pixel/PIXEL-CHAIN-D-P01-D-P07.md` (parallel UI build)
- Atlas chain: `../EuriFlow/prompts/ATLAS-CHAIN-M3-M10.md` (backend build)
