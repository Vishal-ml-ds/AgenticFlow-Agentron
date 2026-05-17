<div align="center">

# AGENTRON

### AI Business Operations OS — *Workflows that work themselves.*

[![Status](https://img.shields.io/badge/status-pre--launch-blue)](#project-status)
[![Build](https://img.shields.io/badge/build-M3%20in%20progress-orange)](#build-progress)
[![License](https://img.shields.io/badge/license-AGPL%20v3%20%2B%20Commercial-green)](./LICENSE.md)
[![Course](https://img.shields.io/badge/euron-Project%202-purple)](https://euron.one/course/ai-product-engineering-2-0)
[![Launch](https://img.shields.io/badge/launch-Jun%2010%2C%202026-red)](#timeline)

</div>

---

## Elevator Pitch

**Agentron is an AI Business Operations OS** that completes sales, support, and finance workflows across your existing tools — without you wiring nodes, debugging payload schemas, or babysitting runs.

Three product lines (**AI Sales Desk** · **AI Support Desk** · **AI Ops Desk**) on a generic workflow builder. **Five AI agents** under a LangGraph supervisor (Architect / Executor / Observer / Repair / Orchestrator) design, run, observe, and self-heal each workflow. Multi-tenant SaaS from day one. Open-core (AGPL v3 + Commercial Exception).

You describe a workflow in plain English **or** drag blocks onto a canvas. The Architect agent emits a `chains.json` graph. The Executor runs each step through the MCP tool layer. The Observer watches every call for schema drift. When something breaks (HubSpot adds a field, Stripe deprecates a parameter), the **Repair agent** diagnoses, patches, sandbox-replays, and deploys the fix — autonomously for safe categories, with human approval for everything else. Every action lands in an immutable audit log with 7-year retention.

---

## Project Status

> **Euron AI Product Engineering 2.0 · Project 2 of 10 · Live build**

| Field | Value |
|-------|-------|
| **Course** | [AI Product Engineering 2.0](https://euron.one/course/ai-product-engineering-2-0) (Euron, Sudhanshu) |
| **Lead** | Dhruv Tomar — PRD + Architecture + Repo + Docs |
| **Team** | 16 team members across India / Nigeria / Senegal / US |
| **Build window** | May 11 → Jun 10, 2026 (5-week sprint) |
| **Launch dates** | Product Hunt Jun 10 · Show HN Jun 12 |
| **Repository** | [euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine](https://github.com/euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine) |
| **Project tracker** | [Master Allocation Sheet (Google Sheets)](https://docs.google.com/spreadsheets/d/1lBFUPR-p2jHxcomAlp5SACEHBlAzrFOE/edit?gid=1835051220) |
| **WhatsApp group** | [Team chat](https://chat.whatsapp.com/IZjABafSeaW8TqUiAojoqo) |

---

## Repository Architecture

This public repository contains the **architecture, plan, methodology, and governance** for Agentron. The source code lives in a **private repository** during the 5-week build window (May 14 → Jun 10, 2026) and merges back here as **v1.0.0 at launch**.

| Layer | Where | What's there |
|-------|-------|--------------|
| **Public (this repo)** | `euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine` | README · CLAUDE.md · STRUCTURE.md · ROADMAP.md · ERRORS.md · LICENSE.md · CHANGELOG.md · `agents/` · `rules/` · `methodology/` · `docs/` (BRD, PRD, Architecture, LLD, launch artifacts) |
| **Private** | `aiagentwithdhruv/agentron` (invite-only) | The codebase — `apps/`, `packages/`, `supabase/`, `inngest/`, migrations, tests |
| **Local backup** | Maintainer's machine | Frozen snapshot of every state, including uncommitted work-in-progress |

**Why split?** The Agentron *methodology* (AI agents building software via dispatch-flow + Definition-of-Done gates) is the IP worth sharing publicly. The half-finished code during the build adds noise, not signal. Reviewers see a clean blueprint; the team ships in a working repo without the pressure of every WIP commit being world-readable.

**At launch:** the private code merges into this repo as a single `v1.0.0` commit, and this repo flips from blueprint to canonical source.

---

## The Problem

Sales, support, and finance teams spend ~60% of their time on repeatable plumbing — qualifying inbound leads, updating CRM fields, answering the same FAQs fifty times a day, triaging tickets, matching invoices to POs, chasing approvals. The work has clear rules but still requires a human keystroke.

Existing tools fall short on different axes:

| Category | Examples | Limitation |
|----------|----------|------------|
| Drag-drop iPaaS | Zapier · Make · n8n | Humans hand-wire every node, hand-fix every schema break |
| Agent libraries | LangGraph · CrewAI | Libraries, not products — non-engineers can't use them |
| CRM automations | HubSpot · Salesforce | Locked inside one tool, no cross-system orchestration |
| Support chatbots | Intercom Fin · Zendesk AI | Answer questions but don't execute workflows or update systems |
| Vertical SaaS | (5+ separate tools per function) | None talk to each other, expensive at scale |

**Agentron's bet:** the workflow execution layer becomes agentic. You describe outcomes, agents design + run + self-heal the plumbing.

---

## What Makes Agentron Different

| # | Differentiator | Why competitors can't easily copy |
|---|----------------|-----------------------------------|
| 1 | **Self-healing workflows** | Repair Agent diagnoses schema drift, patches the workflow, sandbox-replays, and deploys the fix without human touch. Requires reflexion-loop architecture + sandboxed re-execution + workflow versioning — a deep engineering moat. |
| 2 | **NL + drag-drop dual entry** | Type a sentence *or* drag blocks — same canvas, same runtime. Zapier/Make are drag-drop only; LangGraph is code-only. |
| 3 | **Three desks on one runtime** | Sales, Support, and Ops share the workflow builder, knowledge layer, integrations, and governance. Vertical SaaS players each only do one. |
| 4 | **Multi-tenant from day one** | Agency operators run 10–50 client workspaces from one account. LangGraph/CrewAI are libraries, not multi-tenant products. |
| 5 | **AI Control Tower** | Real-time approval inbox + cost monitor + 4-level kill switch + immutable audit logs. Most agent platforms ship the runtime first and add governance later — we ship it with v1. |
| 6 | **Open-core (AGPL + Commercial)** | Community grows the integrations long tail; commercial license protects revenue. n8n proved the pattern — we apply it to agentic, not deterministic, workflows. |
| 7 | **Mobile-first approval flow** | Push to phone → tap approve → run continues. Web-only competitors lose the after-hours approval window. |

---

## Core Features (16 across 4 categories)

Sized for MVP launch on Jun 10, 2026. Day-1 templates ship alongside (20 ready-to-use workflows).

### Sales Desk (5)

| # | Feature | What it does |
|---|---------|--------------|
| F1 | **Inbound Lead Qualification** | Form/email/chat input → AI scores lead against ICP rubric → routes to AE or nurture (60s p95) |
| F2 | Lead Enrichment | Perplexity-grounded research pulls company size, industry, funding, tech stack |
| F3 | ICP Scoring (configurable rules) | Rule editor: "Score 90+ if SaaS, 50-200 employees, Series A+" — saved per workspace |
| F4 | Personalized Outreach + Sequences | First email drafted + multi-step follow-ups, auto-stop on reply |
| F5 | Proposal / Quote Draft | PDF generated from CRM data + pricing rules + workspace template |

### Support Desk (5)

| # | Feature | What it does |
|---|---------|--------------|
| F6 | Ticket Triage + Priority Detection | Inbound ticket → AI classifies category + urgency → routes to right team |
| F7 | FAQ Auto-Resolution from Notion KB | AI searches Notion-mirrored KB → replies with source citation if confident |
| F8 | Sentiment-Based Escalation | Detects frustrated customers → escalates to senior agent with full context |
| F9 | Suggested Agent Replies | Drafts a reply for human agents to review / edit / send |
| F10 | Refund / Cancellation Processing | Customer requests refund → AI checks policy → approves or escalates |

### Ops Desk (3 — Finance-narrowed)

| # | Feature | What it does |
|---|---------|--------------|
| F11 | Invoice OCR + Extraction | Inbound invoice (PDF, email) → OCR → structured data (vendor, amount, line items) |
| F12 | Invoice-to-PO Matching | Match invoice against Zoho ERP PO records → flag mismatches with diff |
| F13 | Stripe Payment Triggering | After approval → trigger Stripe payment → update Zoho ERP → confirmation email |

### Common Platform (3)

| # | Feature | What it does |
|---|---------|--------------|
| F14 | **AI Control Tower** (Admin Console) | Agent registry · workflow registry · approval inbox · action logs · cost monitor · health monitor · kill switch · policy manager · ROI dashboard |
| F15 | Multi-Tenant Workspaces | Single account → multiple isolated workspaces (e.g., agency with 10 clients). RLS-enforced. |
| F16 | Audit Logs (Immutable) | Every action by every agent → append-only log → 7-year retention · monthly partitions · searchable |

---

## Architecture

### High-Level System Diagram

```
┌─────────────────────────────────────────────────────────┐
│ CLIENTS                                                  │
│ Web (Next.js 16) · Mobile (RN + Expo + EAS) · Admin     │
└────────────────────────┬────────────────────────────────┘
                         │ HTTPS / WSS
                         ▼
┌─────────────────────────────────────────────────────────┐
│ API GATEWAY — Vercel Edge + Cloudflare                  │
└────────────────────────┬────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│ BACKEND — Next.js 16 API Routes + Zod                   │
│ Auth · RBAC · Tenancy resolution · Webhook ingress      │
└────────┬────────────────────────────────────┬───────────┘
         │                                    │
         ▼                                    ▼
┌─────────────────┐               ┌────────────────────────┐
│ INNGEST         │               │ LANGGRAPH SUPERVISOR    │
│ Workflow runtime│◄──────────────┤ ┌───────────────────┐  │
│ Durable / retry │               │ │  ORCHESTRATOR     │  │
└────────┬────────┘               │ │  (state machine)  │  │
         │                        │ └────────┬──────────┘  │
         │                        │    ┌─────┼─────┐       │
         │                        │    ▼     ▼     ▼       │
         │                        │ Architect Exec Observer│
         │                        │              │  │      │
         │                        │              │  ▼      │
         │                        │              │ Repair  │
         │                        │              │ (reflex)│
         │                        │              └──┘      │
         │                        └────┬─────────────┬─────┘
         │                             │             │
         ▼                             ▼             │
┌─────────────────────────────────────────┐          │
│ MCP TOOL LAYER (fastmcp) + Composio     │◄─────────┘
│ Gmail · HubSpot · Slack · Zoho · Stripe │
│ Calendly · Notion · Freshdesk + 1000+   │
└────────────────────┬────────────────────┘
                     │
         ┌───────────┼────────────┐
         ▼           ▼            ▼
   ┌──────────┐ ┌─────────┐ ┌──────────┐
   │ Supabase │ │ Upstash │ │   S3     │
   │ Postgres │ │  Redis  │ │ artifacts│
   │ pgvector │ │ 24h TTL │ │ + audit  │
   │ RLS Auth │ │         │ │ archive  │
   └──────────┘ └─────────┘ └──────────┘
         ▲
         │ aggregates
┌────────┴────────────────────────┐
│ AI CONTROL TOWER (admin UI)     │
│ Approval inbox · cost monitor   │
│ 4-level kill switch · audit log │
│ ROI dashboard · policy editor   │
└─────────────────────────────────┘
```

**One-page architecture poster (PDF):** [docs/Agentron-Architecture-v1.1.pdf](./docs/Agentron-Architecture-v1.1.pdf)

### The 5 Agents (LangGraph supervisor)

| Agent | Role | Reasoning Pattern | Approval boundary |
|-------|------|-------------------|-------------------|
| **Orchestrator** | Supervisor / state machine, routes work between the others | Deterministic transitions | none (control plane) |
| **Architect** | Designs the workflow graph from NL prompt or template | Planning (full graph upfront) | none (planning only) |
| **Executor** | Runs each step via MCP tool calls | Reactive (one step at a time) | bounded by per-workflow autonomy config |
| **Observer** | Watches every run for failures + schema drift | Read-only telemetry | none (read-only) |
| **Repair** | Diagnoses failures, patches workflow, sandbox-replays | Reflexion loop | auto for schema-drift; approval for semantic |

### Workflow State Machine

```
idle → planning → awaiting_review → executing → (awaiting_approval | repairing | done | escalated)
```

Every transition is audit-logged. Risk engine routes approval-required steps to the AI Control Tower inbox + mobile push.

### 6-Step Tool-Call Gate

Every external tool invocation passes through six checks:

1. **Workspace allowlist** — is this tool connected in this workspace?
2. **Per-agent allowlist** — can this agent slug call this tool?
3. **Autonomy config** — does this action need approval?
4. **Schema validation** — does input match the tool's schema?
5. **Cost cap** — would this push the run over budget?
6. **Idempotency key** — is this a retry of an already-executed call?

### 6-Layer Guardrails

Policy · Input (prompt injection + PII redaction) · Retrieval (authz + citations) · Tool/Action (whitelists + dry-run) · Output (factuality + compliance) · Monitoring (cost spikes + red-team tests).

---

## Technology Stack

> Stack locked 2026-05-14 per PRD §25 + LLD §1. TypeScript-first on Vercel; Hetzner deferred until a Python-only lift forces it.

| Layer | Choice | Why |
|-------|--------|-----|
| Monorepo | Turborepo + pnpm workspaces | Standard for Vercel apps |
| Web frontend | Next.js 16 + TypeScript 5 + Tailwind 4 + Radix UI | Fast SSR, App Router maturity, accessibility primitives |
| Mobile | React Native + Expo + EAS | Single codebase iOS+Android; EAS removes native build pain |
| Backend | Next.js 16 API routes | One deploy, vendor-managed scaling, async-first |
| Workflow runtime | **Inngest 3.54+** | Durable execution, retry-safe, sleep-resumable |
| Agent framework | **LangGraph (`@langchain/langgraph`)** | Supervisor pattern + state machines |
| Tool layer | **MCP (`@modelcontextprotocol/sdk`) + Composio** | Standard tool-calling + 1,000-app long tail |
| LLM routing | **Euri Gateway** → OpenRouter → Anthropic / OpenAI direct | Cost-optimized; multi-provider fallback |
| Database | **Supabase** (Postgres 15 + pgvector + Auth + RLS) | Single source for relational + vector + auth |
| Cache + queue | Upstash Redis | Serverless, Vercel-native, zero-ops |
| Storage | Supabase Storage (S3-compatible) | One bill, one auth |
| Email | Resend | Developer-first transactional |
| Payments | Stripe | Subscription + Connect for outbound payments |
| Voice (Sales onboarding only) | Gemini Live API | Production-ready streaming voice |
| Observability | Sentry + Langfuse (AI traces) + Vercel Analytics | Errors + LLM traces + metrics |
| IaC | Terraform (cloud) — local dev uses Docker | Multi-cloud capable |
| CI/CD | GitHub Actions | Lint + typecheck + build on every PR |
| License | AGPL v3 + Commercial Exception | MongoDB open-core pattern |

### LLM Routing (Euri Gateway)

| Use case | Primary | Fallback |
|----------|---------|----------|
| Classification (intent, sentiment) | Claude Haiku 4.5 | Gemini Flash 3 |
| Reasoning (Architect graph generation, Repair diagnosis) | Claude Opus 4.7 / Sonnet 4.6 | GPT-4o |
| Embeddings | OpenAI `text-embedding-3-small` (1536d) | Voyage AI |
| Reranking (KB hybrid search) | Cohere Rerank | Voyage Rerank |

---

## Getting Started

> **Prerequisites:** macOS / Linux · Node 22+ · pnpm 9+ · Docker Desktop · Supabase CLI · gh CLI · ~3 GB free disk

```bash
# 1. Clone the repo
gh repo clone euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine agentron
cd agentron
git checkout develop

# 2. Install dependencies
pnpm install

# 3. Start Supabase local stack (Postgres + Auth + Storage + Mailpit)
npx supabase start

# 4. Apply migrations
npx supabase db reset

# 5. Copy env template + fill in any keys you have (LLM keys can come later)
cp .env.example apps/web/.env.local
# Edit apps/web/.env.local — Supabase URLs auto-fill from `npx supabase status -o env`

# 6. Run the dev stack
pnpm dev          # Next.js on :3000

# In a second terminal:
npx inngest-cli@latest dev -u http://localhost:3000/api/inngest

# 7. Open the surfaces
open http://localhost:3000     # The app
open http://localhost:54323    # Supabase Studio (DB admin)
open http://localhost:54324    # Mailpit (catch magic-link emails)
open http://localhost:8288     # Inngest dashboard
```

Sign up at `http://localhost:3000/sign-up` → magic link arrives in Mailpit → click → land on `/dashboard`.

---

## Project Structure

```
agentron/
├── apps/
│   ├── web/                          # Next.js 16 — app + API + landing
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   ├── (marketing)/      # public — landing, pricing, docs
│   │   │   │   ├── (auth)/           # sign-in, sign-up, callback
│   │   │   │   ├── (app)/            # authenticated shell
│   │   │   │   │   ├── dashboard/
│   │   │   │   │   ├── workflows/[id]/
│   │   │   │   │   ├── runs/[id]/
│   │   │   │   │   ├── approvals/
│   │   │   │   │   ├── tools/
│   │   │   │   │   ├── control-tower/
│   │   │   │   │   └── settings/
│   │   │   │   └── api/
│   │   │   │       ├── v1/           # versioned REST surface
│   │   │   │       └── inngest/      # Inngest function registration
│   │   │   ├── components/
│   │   │   ├── lib/
│   │   │   └── middleware.ts         # auth + workspace context
│   │   └── package.json
│   │
│   └── mobile/                       # React Native + Expo (M10)
│       └── src/
│
├── packages/
│   ├── agents/                       # 5-agent LangGraph supervisor
│   │   └── src/
│   │       ├── orchestrator.ts
│   │       ├── architect.ts
│   │       ├── executor.ts
│   │       ├── observer.ts
│   │       └── repair.ts
│   ├── tools/                        # MCP connectors + 6-step gate
│   │   └── src/
│   │       ├── registry.ts
│   │       ├── gate.ts
│   │       └── connectors/{gmail,hubspot,notion,slack,stripe,perplexity,zoho}/
│   ├── workflows/                    # 20 Day-1 workflow templates
│   │   └── src/templates/
│   └── shared/                       # types · Zod schemas · constants
│       └── src/
│
├── supabase/
│   ├── migrations/                   # SQL migrations 001-016
│   ├── seed.sql                      # demo data
│   └── config.toml
│
├── inngest/                          # Inngest function specs (placeholder)
│
├── docs/
│   ├── 01-BRD.md                     # Business requirements
│   ├── 02-PRD.md                     # 31-section PRD (Sudhanshu's 20 + 11 expansions)
│   ├── 03-ARCHITECTURE.md            # High-Level Design
│   ├── 04-LLD.md                     # Low-Level Design (schema + API contract)
│   ├── Agentron-Architecture-v1.1.pdf # One-page architecture poster
│   └── launch/                       # PH + Show HN + YouTube scripts (M10)
│
├── CLAUDE.md                         # Master context for AI agents
├── ERRORS.md                         # Self-learning log (Error → Cause → Fix → Rule)
├── ROADMAP.md                        # 11-milestone tracker
└── README.md                         # This file
```

---

## Documentation (Reviewer Quick Links)

> **For Sudhanshu's review.** Everything graded lives here.

| Document | What it covers | Format |
|----------|----------------|--------|
| **[PRD v1.1 — `Agentron_PRD_v1_1`](https://docs.google.com/document/d/1maSp68WLOrrKRivCqnqO6xhP9OCgasaU6sgTNO4IwQg/edit)** | The full Product Requirements Doc — Sudhanshu's 20-section format + 11 expansions (Market Analysis · GTM Engineering · Product Architecture · Tech Stack · Infra Cost · Compliance · Risks · Open Questions · Conclusion · 7 Pillars Appendix) | Google Doc |
| [docs/02-PRD.md](./docs/02-PRD.md) | Local mirror of the Google Doc (kept in sync) | Markdown · 98 KB · 31 sections |
| [docs/01-BRD.md](./docs/01-BRD.md) | Business Requirements — the "why" before the "what" | Markdown |
| [docs/03-ARCHITECTURE.md](./docs/03-ARCHITECTURE.md) | High-Level Design — system topology, agent contracts, deployment | Markdown |
| [docs/04-LLD.md](./docs/04-LLD.md) | Low-Level Design — every migration, every API route, every Inngest function, DoD checklist | Markdown |
| [docs/Agentron-Architecture-v1.1.pdf](./docs/Agentron-Architecture-v1.1.pdf) | One-page architecture poster (printable A4 landscape) | PDF |
| [Master Allocation Sheet](https://docs.google.com/spreadsheets/d/1lBFUPR-p2jHxcomAlp5SACEHBlAzrFOE/edit?gid=1835051220) | Course-wide tracker — 10 projects, 137 people | Google Sheets |
| [Master PRD Template](https://docs.google.com/document/d/1NxoKGwUCsXIzN0zV0Xud_21zCeYwfg8YjPi7Fpb3N20/edit) | Sudhanshu's canonical 20-section format | Google Doc |

---

## Build Progress

> **One commit per module. Every commit pushed to `develop` branch. `main` is sacred — merged only at launch.**

| # | Milestone | Status | Commit | LOC | Time |
|---|-----------|--------|--------|-----|------|
| M0 | Repo init + monorepo skeleton | ✅ Done | [`b4949c8`](https://github.com/euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine/commit/b4949c8) | 1,035 | ~50 min |
| M1 | Foundation — orgs · workspaces · auth · RLS | ✅ Done | [`3615b14`](https://github.com/euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine/commit/3615b14) | +1,432 | ~11 min |
| M2 | Workflows core — definitions · runs · steps · state machine · Inngest entry | ✅ Done | [`52d8c31`](https://github.com/euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine/commit/52d8c31) | +2,420 | ~45 min |
| M3 | Agents skeleton — LangGraph 5-agent supervisor + Euri LLM routing | 🟡 In progress | — | — | — |
| M4 | Tools registry + Gmail/HubSpot/Notion MCP connectors + Notion KB sync | ⏳ Queued | — | — | — |
| M5 | Approvals inbox + audit log + 4-level kill switch + policies | ⏳ Queued | — | — | — |
| M6 | **F1 Inbound Lead Qualification** (hero feature, end-to-end) | ⏳ Queued | — | — | — |
| M7 | **Repair Agent** (reflexion loop + sandbox-replay — the differentiator) | ⏳ Queued | — | — | — |
| M8 | AI Control Tower + multi-tenant cost monitor + real-time metrics | ⏳ Queued | — | — | — |
| M9 | Support Desk (F6-F10) + Ops Desk (F11-F13) | ⏳ Queued | — | — | — |
| M10 | UI (light-premium) + Mobile (RN+Expo) + Launch artifacts (PH/HN/YouTube) | ⏳ Queued | — | — | — |

**Tests:** 31/31 passing at M2 (8 M1 + 23 M2). Counter updates per module.

---

## Mapping to Sudhanshu's 14-Milestone Tracker

> Per the Master Allocation Sheet's standard tracker (Project 2 tab).

| # | Milestone | Owner | Status |
|---|-----------|-------|--------|
| 1 | Kickoff meeting & team alignment | Lead | ✅ Done |
| **2** | **PRD finalization & sign-off** | **Dhruv (Lead)** | ✅ v1.1 published — awaiting team + Sudhanshu sign-off |
| 3 | GitHub repo setup & access provisioning | Dhruv (Lead) | ✅ Repo live, `develop` branch active, all modules pushed |
| 4 | WhatsApp group + comms cadence set | Lead | ✅ Done |
| **5** | **Architecture & system design doc** | **Dhruv (Lead)** | ✅ HLD + LLD + 1-page PDF poster shipped |
| 6 | Sprint 1 — MVP scope freeze | Lead | ✅ Locked at 16 features (5 Sales / 5 Support / 3 Ops / 3 Platform) |
| 7 | Sprint 1 — Development | Team | 🟡 M3-M10 build chain executing |
| 8 | Sprint 1 — Internal demo & review | Lead | ⏳ After M6 (F1 hero feature) |
| 9 | Sprint 2 — Iteration & feature build | Team | ⏳ M7-M9 |
| 10 | QA / Testing pass | Team | ⏳ End of M9 |
| 11 | Documentation & handover materials | Dhruv (Lead) | 🟡 This README + docs/* (in progress) |
| 12 | Stakeholder review & feedback | Lead | ⏳ Pre-launch (early Jun) |
| 13 | Final integration & deployment | Team | ⏳ M10 launch prep |
| 14 | Project closure & retrospective | Lead | ⏳ Post Jun 10 |

---

## Timeline

| Phase | Window | Deliverables |
|-------|--------|--------------|
| Foundation | May 11-17 | M0-M5: repo + auth + workflows + agents + tools + governance |
| First hero | May 18-24 | M6: F1 Lead Qualification end-to-end · first Internal Demo |
| Differentiator | May 25-31 | M7: Repair Agent + reflexion loop |
| Surface area | Jun 1-7 | M8: Control Tower + M9: Support/Ops desks |
| Launch | Jun 8-10 | M10: UI polish + Mobile + PH submission + Show HN draft + 2 YouTube demo recordings |

---

## The Team

> Lead + 16 team members. Course-wide cohort: 137 people across 10 projects.

| Role | Name | Email |
|------|------|-------|
| **Project Lead** | Dhruv Tomar | aiwithdhruv@gmail.com |
| Team Member | Afeefa Albeena Sheikh | afeefaquadri@gmail.com |
| Team Member | Raunak Ravi | raunakravi084@gmail.com |
| Team Member | Mohammed Junaid Rizvi | rizvi.junaidjmi@gmail.com |
| Team Member | Shawn Smothers | ssmothersbu12@gmail.com |
| Team Member | Vishal Prasad | vishalprasad2442002@gmail.com |
| Team Member | Vikash Kushwaha | ervplji@gmail.com |
| Team Member | Kaushlendra Kumar Gupta | kaushraj1@gmail.com |
| Team Member | Chidi Henry | chidihenry12@gmail.com |
| Team Member | Govardhan M | mlvardhan1@gmail.com |
| Team Member | Yasin Indikar | nizam.niazi8999@gmail.com |
| Team Member | Ramkumar Illa | ramkumar.illa6366@gmail.com |
| Team Member | Debkumar Singha Roy | debkumar.singha8@gmail.com |
| Team Member | Ajinkya Abhang | ajinkyaabhang@gmail.com |
| Team Member | PAPA BA | papamadouba@gmail.com |
| Team Member | Sriram TK | sriram.t.krishnan@gmail.com |
| Team Member | Smita Gavandi | smita.gavandi@gmail.com |
| (+ extended pool) | Imtiaz · Ayush · Purnima · Akash · Sahil · Kinjal · Rohit · Manish · Ramesh · Ankit | … |

**Course instructor:** Sudhanshu Kumar — [Euron AI Product Engineering 2.0](https://euron.one/course/ai-product-engineering-2-0)

---

## Post-Launch Roadmap

Honest tech debt — things deferred from MVP scope:

| Item | Module that owns the work | Why deferred |
|------|---------------------------|--------------|
| Full sandbox isolation for Repair Agent (Docker mock-creds dry-run mode) | M7.1 | Real-creds sandbox ships with MVP; full isolation = 2-week build, post-launch |
| Remaining 17 UI screens fully styled (S8-S24) | M10.1 | 7 priority screens ship at launch; styling pass for support/ops/admin screens post-launch |
| Real Freshdesk + Zoho OAuth flows | M9.1 | Connector stubs ship; production OAuth wires in post-launch |
| Cloud Supabase Pro provisioning | At launch | Local dev throughout build; cloud project provisioned at launch (billing decision) |
| iOS App Store submission | Post-launch (Apple review = 1-3 weeks) | Android-first launch Jun 10, iOS Jun 25 |
| Multi-language ticket support (F6 polish) | Post-launch | English-only at MVP; i18n in v1.1 |
| Voice (Sales onboarding) via Gemini Live | Post-launch | Cut from MVP — adds 1 week, demos poorly on PH |
| SSO (SAML / OIDC) | Post-launch | Email + Google + GitHub OAuth at launch; enterprise SSO at v1.2 |
| Public API + webhooks for external consumers | Post-launch | Internal API stable at launch; public-facing API contract in v1.2 |

---

## License

**AGPL v3 + Commercial Exception** (open-core, MongoDB pattern).

Use Agentron under AGPL v3 for personal, internal, or open-source use. If you want to ship a closed-source product *built on top of Agentron* (white-label, SaaS resale, embed without code disclosure), you need the **Commercial License** — contact aiwithdhruv@gmail.com.

Full text: [LICENSE.md](./LICENSE.md) (lands at M10).

---

## Contact

- **Maintainer:** Dhruv Tomar — [aiwithdhruv.com](https://aiwithdhruv.com) · aiwithdhruv@gmail.com
- **Course:** [Euron AI Product Engineering 2.0](https://euron.one/course/ai-product-engineering-2-0)
- **Demo:** [agentron.site](https://agentron.site) (live post-launch)
- **Twitter / X:** [@Aiwithdhruv](https://twitter.com/Aiwithdhruv)
- **LinkedIn:** [aiwithdhruv](https://linkedin.com/in/aiwithdhruv)

---

<div align="center">

**Built with the [Dhruv 65 Production Rules](https://github.com/aiagentwithdhruv/ai-methodology-v1) operating doctrine.**
**PM: Angelina · Engineer: Atlas · Approver: Dhruv.**

*Last updated: 2026-05-14 · Build at M3*

</div>
