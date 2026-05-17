# Changelog

All notable changes to Agentron land here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Pixel UI chain D-P01 through D-P07 — marketing, auth, dashboard,
  approvals + tools + control-tower, three desks, settings + audit, mobile.

### Tech debt to address before v1.1
- Materialized view `workflow_metrics_24h` (Control Tower aggregations
  currently live-query).
- Full sandbox isolation for Repair agent (M7.1 — see CLAUDE.md).
- Freshdesk + Zoho OAuth flows (API-key auth ships in v1.0).

---

## [1.0.0] — 2026-06-10 — Public launch

Five-week MVP sprint complete. Free + Pro + Team + Business + Lifetime
tiers live. Product Hunt + Show HN coordinated launch.

### Atlas backend chain (M0 → M10)

| Module | Commit | Summary |
|--------|--------|---------|
| M0 | `b4949c8` | Monorepo skeleton (apps/web + packages + supabase + inngest + docs) |
| M1 | `3615b14` | Foundation — orgs + workspaces + memberships + RLS + auth + 5 API routes |
| M2 | `52d8c31` | Workflows core — definitions + runs + steps + state machine + Inngest entry |
| M3 | `e9613a5` | Agents skeleton — LangGraph 5-agent supervisor + Euri Gateway LLM routing |
| M4 | `b12eae2` | Tools + KB — registry + 6-step gate + Gmail/HubSpot/Notion + hybrid search |
| M5 | `6abcf29` | Approvals + audit + policies + kill switch (4-level governance plane) |
| M6 | `aab85ce` | F1 Inbound Lead Qualification — hero feature E2E |
| M7 | (inside `78bfbc3`) | Repair agent — reflexion loop + sandbox-replay + auto-deploy gate |
| M8 | `dc7acd3` | Control Tower + cost ledger + realtime metrics |
| M9 | `d5a8a29` | Support Desk F6–F10 + Ops Desk F11–F13 (8 features) |
| M10 | (this commit) | Launch artifacts — LICENSE + PH/HN/YT scripts + DEPLOY + CHANGELOG |

### Pixel UI chain (D-P01 → D-P07)

| Module | Commit | Summary |
|--------|--------|---------|
| D-P01 | `1673f91` | Design system + marketing site (8 routes + 7 UI primitives) |
| D-P02 | `1203041` | Auth + onboarding (split-screen sign-in/up + 3-step wizard) |
| D-P03 | (Pixel) | Dashboard shell + sidebar + topbar (deferred surface in 78bfbc3 series) |
| D-P04 | `29e0bea` | Approvals + tools + control tower (3 routes) |
| D-P05 | `f9d2cc8` | Three desks — Sales 4 + Support 4 + Ops 3 (11 routes) |
| D-P06 | `78bfbc3` | Settings (6 tabs) + audit log search + polish |
| D-P07 | `2a44edd` | Mobile app scaffold (Expo Router + 5 screens) |

### Features shipped

| # | Feature | Status |
|---|---------|--------|
| F1  | Inbound Lead Qualification | Production |
| F2  | Outbound Sequencer         | Stubbed via `outreach_sequences` table — sequencer runs in v1.1 |
| F3  | Quote Generator            | Deferred to v1.1 |
| F4  | CRM Sync                   | Production via HubSpot connector |
| F5  | Sales Coaching             | Deferred to v1.1 |
| F6  | Ticket Triage              | Production |
| F7  | FAQ Resolve                | Production |
| F8  | Sentiment Escalate         | Production |
| F9  | Multi-channel Ingest       | Production for email + form + api; whatsapp/phone in v1.1 |
| F10 | Refund Process             | Production |
| F11 | Invoice OCR + Extract      | Production (Textract scaffolded, Mathpix fallback wired) |
| F12 | PO Match                   | Production (Zoho Books) |
| F13 | Invoice Approval + Pay     | Production (Stripe live) |
| F14 | Control Tower              | Production |
| F15 | Builder Canvas             | Production (Pixel D-P03+) |
| F16 | Mobile Approval Inbox      | Production (Pixel D-P07) |

### Migrations

`supabase/migrations/`:
- 001 organizations · 002 workspaces · 003 memberships
- 004 workflows · 005 workflow_runs · 006 workflow_steps
- 007 agents_registry · 008 tools_registry · 008b knowledge_sources
- 009 approvals · 010 audit_log · 011 policies
- 012 leads · 013 sales_tools_seed
- 014 repair_attempts · 015 cost_ledger
- 016 tickets · 017 invoices

### Tests

`apps/web/src/__tests__/` — 10 test files, 148 tests passing:
- m1-foundation (8), m2-workflows (22), m3-agents (18), m4-tools-kb (13),
  m5-approvals-audit (10), m6-leads (17), m7-repair (23),
  m8-control-tower (10), m9-support-ops (17), d-p02-auth (10).

### License

AGPL-3.0-or-later WITH Agentron-Commercial-Exception. See `LICENSE.md`.
