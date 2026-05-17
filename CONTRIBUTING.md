# Contributing to Agentron

> **Audience:** Anyone on the Euron AI Product Engineering 2.0 team (or wider community) who wants to push docs, fixes, or content into this public repo.
> **What lives here:** Architecture, plan, methodology, governance, docs. The actual code is in a separate private repo and merges back at launch as `v1.0.0`. See README §"Repository Architecture".

---

## Branches

| Branch | Purpose | Who pushes |
|--------|---------|------------|
| `develop` | Canonical state. Reviewed + merged. | Lead (Dhruv) — merges PRs |
| `team-contributions` | Open lane for the team. Anyone can push here, no PR required. | Anyone on the team |
| Feature branches (`docs/<slug>`, `fix/<slug>`) | One-off work that needs its own PR | Anyone — PR into `develop` |

There is **no `main` branch** during the build. `main` arrives at launch (Jun 10, 2026) when the private code merges in as `v1.0.0`.

---

## Workflow

### Option A — Open lane (`team-contributions`) — fastest

For docs edits, README polish, small fixes, additional notes.

```bash
# Clone (first time only)
git clone https://github.com/euron-sudh/PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine.git
cd PROJECT-2-EuriFlow-Autonomous-AI-Workflow-Engine

# Get on the open lane
git fetch origin
git checkout team-contributions
git pull --rebase origin team-contributions

# Make your edit
# (edit files)
git add <file>
git commit -m "docs: <what you changed in one line>"
git push origin team-contributions
```

Done. Dhruv reviews + merges `team-contributions` → `develop` weekly (or sooner for time-sensitive changes).

If someone else pushed while you were working, do `git pull --rebase origin team-contributions` before pushing.

### Option B — Feature branch + PR — for bigger changes

For new docs, restructures, anything that benefits from review before landing.

```bash
git checkout develop
git pull --rebase origin develop
git checkout -b docs/<your-slug>
# (edit files)
git add <file>
git commit -m "docs: <what>"
git push origin docs/<your-slug>
gh pr create --base develop --title "docs: <what>" --body "<why + what changed>"
```

Dhruv reviews + merges.

---

## What to push

| Yes |
|-----|
| Edits to README, CLAUDE.md, ROADMAP.md, STRUCTURE.md |
| Docs under `docs/` (BRD, PRD, Architecture, LLD) — typo fixes, additional sections, examples |
| Launch artifacts under `docs/launch/` — PRODUCT-HUNT, SHOW-HN, DEPLOY, FINAL-STATUS, YOUTUBE scripts |
| Methodology + governance edits under `agents/`, `rules/`, `methodology/` |
| New CONTRIBUTING / SECURITY / CODE_OF_CONDUCT files |

| No |
|----|
| Application code (`apps/`, `packages/`, `supabase/`, `inngest/`) — those live in the private repo |
| Build configs (`package.json`, `vercel.json`, `turbo.json`, lockfiles) |
| Anything that requires running the app locally — go to the private repo |

If you need access to the private repo to ship code, ask Dhruv directly.

---

## Commit message format

```
<type>: <subject line, ≤72 chars>

<optional body explaining the why>
```

Types: `docs` · `chore` · `fix` (for typos / docs bugs) · `style` · `content`

Examples:

- `docs: add wireframe links to PRD §16`
- `fix: typo in DEPLOY.md step 4`
- `content: add YouTube demo 1 outline`
- `docs: restructure ROADMAP into 4 phases`

---

## Style

- Markdown, GitHub-flavored.
- Use existing files as your style guide. Don't reinvent.
- Keep tone factual + concise. No marketing copy unless you're editing `docs/launch/`.
- Use the brand palette (cyan `#18D1E0`, deep dark, metallic grey) in any new graphics. No teal, no orange, no red.
- No emojis in product copy. Sparingly in launch comms.

---

## What happens after you push to `team-contributions`

1. CI runs lint on markdown (when configured)
2. Dhruv reviews the diff
3. Dhruv merges into `develop`
4. If anything needs change, Dhruv comments + you push a fix to the same branch
5. After merge, `team-contributions` is reset to match `develop` weekly so it stays clean

If you don't hear back in 48 hours: drop a `@Dhruv Tomar` ping in the WhatsApp team chat.

---

## Questions

- Can't push? Ask Dhruv to add your GitHub handle as a collaborator on this repo.
- Need access to the code repo? Ask Dhruv.
- Course-level questions: Sudhanshu, Sunday 7 PM IST class.

— Dhruv Tomar (Lead) · 2026-05-14
