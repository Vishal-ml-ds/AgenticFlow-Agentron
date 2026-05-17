# Agentron — Repo Structure Guide

> How this repo is organized + who works on what.

## Two-layer organization

Agentron is built by **3 AI agents** following a **strict dispatch flow**. The repo reflects both the product code AND the governance system that produces it.

```
agentron/
├── apps/                    PRODUCT CODE — web + mobile apps
├── packages/                PRODUCT CODE — shared agents, tools, workflows, types
├── supabase/                PRODUCT CODE — DB migrations + config
├── inngest/                 PRODUCT CODE — workflow runtime specs
├── docs/                    SPEC — BRD, PRD, Architecture, LLD
│
├── agents/                  GOVERNANCE — AI agent roles + boundaries
├── rules/                   GOVERNANCE — production rules + branch strategy
├── methodology/             GOVERNANCE — build flow (PRD→HLD→LLD→Build)
│
├── CLAUDE.md                ENTRY — master context (every AI agent reads this first)
├── README.md                PUBLIC — for Sudhanshu + visitors
├── STRUCTURE.md             This file
├── ROADMAP.md               TRACKER — 11 modules M0-M10
├── ERRORS.md                LEARNING — bugs + fixes (Error→Cause→Fix→Rule→Applies-to)
├── CONTRIBUTING.md          (post-launch) how to contribute
└── LICENSE.md               (M10) AGPL v3 + Commercial Exception
```

## How to read this repo

| If you are… | Start here |
|-------------|-----------|
| **Sudhanshu (instructor)** | `README.md` → `docs/02-PRD.md` → `docs/03-ARCHITECTURE.md` → `docs/Agentron-Architecture-v1.1.pdf` (1-page poster) |
| **Future contributor** | `README.md` → `STRUCTURE.md` (this) → `CONTRIBUTING.md` → `agents/README.md` |
| **AI agent (Atlas / Pixel / Angelina)** | `CLAUDE.md` → `agents/<your-name>.md` → `rules/DHRUV-RULES.md` → `methodology/PRD-HLD-LLD.md` |
| **End user** | `README.md` → live demo at [agentron.site](https://agentron.site) |
| **Reviewer / auditor** | `docs/` for specs, `rules/` for rules, `ERRORS.md` for what we learned |

## Code structure (monorepo via Turborepo + pnpm workspaces)

```
apps/
├── web/                          Next.js 16 — landing, auth, app, API
│   └── src/
│       ├── app/
│       │   ├── (marketing)/      Landing, pricing, docs, changelog ← Pixel
│       │   ├── (auth)/           Sign-in, sign-up, callback ← Pixel + Atlas
│       │   ├── (app)/            Dashboard, workflows, etc ← Pixel pages, Atlas API
│       │   └── api/v1/           REST endpoints ← Atlas
│       ├── components/           ← Pixel (shadcn-based)
│       ├── lib/                  ← Atlas (Supabase, Inngest, LLM)
│       └── middleware.ts         ← Atlas
└── mobile/                       React Native + Expo ← Pixel

packages/
├── agents/                       5 LangGraph agents ← Atlas
├── tools/                        MCP connectors + 6-step gate ← Atlas
├── workflows/                    20 day-1 workflow templates ← Atlas
└── shared/                       Types + Zod schemas (API contract) ← Atlas writes, Pixel reads
```

## Governance structure (THE AI factory pattern)

```
agents/                           Defines WHO does what
├── README.md                     The 3-agent model
├── angelina.md                   PM agent (writes dispatches, owns plumbing)
├── atlas.md                      Backend engineer agent
├── pixel.md                      Frontend engineer agent
└── coordination.md               How Atlas + Pixel work in parallel without conflict

rules/                            Defines HOW we work
├── README.md
├── DHRUV-RULES.md                The 65 production rules (sanitized for public)
├── BRANCH-STRATEGY.md            Develop-only until launch
└── DISPATCH-FORMAT.md            D-NNN file format spec

methodology/                      Defines the BUILD FLOW
├── README.md
├── PRD-HLD-LLD.md                The spec → build cascade (R2)
├── DISPATCH-FLOW.md              How PM → Engineer paste works
└── DOD-CHECKLIST.md              Definition of Done per module
```

## How a change reaches production

1. **PM (Angelina)** writes a dispatch file in `prompts/D-NNN-*.md` (this lives in a separate context repo, not committed here)
2. **Engineer (Atlas or Pixel)** reads the dispatch + relevant files in `docs/`, `rules/`, `agents/`
3. Engineer commits work to `develop` branch following the rules
4. PM verifies STATUS against `methodology/DOD-CHECKLIST.md`
5. At launch, PM merges `develop` → `main` and tags `v1.0.0`
6. Vercel auto-deploys `main` → [agentron.site](https://agentron.site)

## What's NOT in this repo

- The dispatches Angelina writes (live in private operating system)
- The full Dhruv-65 rules with team-specific items (lives in private operating system)
- Memory files (each AI agent's private context)
- Internal team coordination (WhatsApp, sheets)

What IS in this repo: everything needed to **read the code and understand how it was built**.
