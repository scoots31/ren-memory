# Pending Work

**Last updated:** 2026-05-07

Items that are open and need to be picked up. Remove when done, add when identified.

---

## Shipped 2026-05-03

- [x] Memory repo setup — ren-memory created, context.md + pending.md + curator-summary.md
- [x] Session-start fetch wired into Ren skill
- [x] Curator-context summary layer (curator-summary.md)
- [x] Cloudflare auto-deploy hook (Stop hook)
- [x] Scheduled maintenance agent — daily 8am, macOS notification, working
- [x] Comms cascade sub-agent — shipped v2.0.0, first real run successful
- [x] Sub-agent wiring — design-review, phase-test, design-sprint, solo-build — v2.1.0 shipped
- [x] Parallel pipeline design session — model fully designed, logged in shared/ideas.md
- [x] Companion board brainstorm — doc written, committed, new cycle ready to open

---

## Shipped 2026-05-05

- [x] v2.4.0 — Design Identity (design library 1322 styles, search.py, extract.py, design-sprint Step 2, design-review north star check)
- [x] Weekly Anthropic release notes monitor (scheduled task, ren-memory/release-notes-scan.md)
- [x] v2.3.0 — Security classification (6 classes, tech-context through Check 9)
- [x] Solo Companion viewer JS bug fix — slice overlay ternary syntax error
- [x] v2.5.0 — Solo Companion Board View (kanban, local + cloud, Phase 6 complete)
- [x] Design library bundled subset — 500 entries (25MB) shipped in repo via trim.py; full 1322 gitignored
- [x] Public APIs reference — tech-context Q5 silent check before any external API dependency
- [x] Comms cascade — v2.4.0 Design Identity + v2.5.0 Board View blog entries live
- [x] Han Solo — strategic product vision documented in shared/ideas.md + MemPalace, 4 design sessions defined
- [x] Marketing workflow idea — documented in shared/ideas.md + MemPalace
- [x] curator-summary.md — updated to v2.4.0

## Framework — Needs Design Session

- [ ] Parallel pipeline framework skills — start soft prompt, Pipeline mode field in handoff.md, records-spec update. Design logged in shared/ideas.md. Execute after companion board ships or independently.
- [ ] prd-to-plan fold — design session required, linked to parallel pipeline item (shared/ideas.md)

## ✅ Build Phase Overhaul — SHIPPED v2.7.0 (2026-05-08)

solo-build fully rewritten + new review-agent skill. All structural changes executed:
- Design+data correlation gate — hard stop before branch opens
- Builder QA manifest (observations only, no self-cert, no done declaration)
- Review Agent owns Done gate — independent, reads everything from source, CLEARED or GAPS only
- Terminal command rule — builder never asks solo to run commands
- Preview port fix — one source of truth (launch.json)
- "Not a blocker" not a valid output from Review Agent

Quick fixes also done:
- prd-to-plan + records-spec.md exact header strings — SHIPPED (2026-05-08)
- All 5 project backlogs verified and corrected (gift-tracker was wrong, fixed)

## Framework — Needs Design Session

- [ ] Parallel pipeline framework skills — start soft prompt, Pipeline mode field in handoff.md, records-spec update. Design logged in shared/ideas.md. Execute after companion board ships or independently.
- [ ] prd-to-plan fold — design session required, linked to parallel pipeline item (shared/ideas.md)
- [ ] Discovery gap fix — for data-heavy products (plant lists, catalogs, curated libraries), discovery must produce an exact data inventory before data collection starts. Currently discovery only captures categories. Needs a gate and a template for "exact records list" as a discovery output.

---

## Framework — Approved, Ready to Execute

- [x] Start: lighter re-entry read — shipped v2.6.1 (2026-05-07)
- [ ] Discover: lighter path for clear problems (shared/ideas.md)
- [ ] Product-continuity: lazy docs — check sync.py + parsers.py first (shared/ideas.md)

---

## Framework — Deferred / Backlog

- [x] solo-build: session limit guardrail — closed 2026-05-07, premature without session telemetry data to set meaningful threshold
- [x] retro: curator path — closed 2026-05-07, retro log reviewed and found empty; reopen when log accumulates signal across multiple projects
- [x] deploy: auto-detect platform — closed permanently 2026-05-07, explicit platform selection at tech-context is the right UX; auto-detect adds convenience but removes a useful forcing function

---

## Solo Companion — New Cycle Ready

- [x] Board view (kanban) — shipped v2.5.0, Phase 6 complete 2026-05-05

---

## Han Solo (Framework as Owned Application)

Full design reference: `ren-memory/han-solo.md` — read this before any design session.

- [x] Design session 1 — Module inventory (completed 2026-05-11)
- [x] Design session 2 — Application architecture (completed 2026-05-11)
- [ ] Design session 3 — Interface design (next)
- [ ] Design session 4 — Collaboration model
- [ ] Dreaming access — Scott trying X outreach to Anthropic (form rejects ProtonMail + Gmail). Not a blocker. Homegrown Dreaming via Letta sleeptime agents in the meantime.
- [ ] Execution environment decision — local vs cloud vs hybrid. May need to resolve before Session 3 interface design.

To open: say "Han Solo" or "open Han Solo" to Ren.
Private to Scott and Ren only — never referenced in engineering-playbook.

---

## Apps (~/Apps)

- [ ] Hair Stylist App — blocked on user interviews; interview guide needed when ready
- [x] Fantasy Football Tool — **deployed live** (2026-05-10). Open: is_scott flag, PFF upload, contracts sync (nfl_data_py py313 incompatible)
- [ ] Chase the Light SwiftUI product — in design phase

## Garden Planner — Data Re-seed Blocked on Plant List

Full data audit done 2026-05-08. Current state:
- 26 plants have broken image_url (Perenual paywall `upgrade_access.jpg`)
- 5 plants typed as `other` — invisible in library browse
- 43 plants missing `indoor_weeks_before_frost` (no calendar dates possible)
- Wrong plants in DB: squirting cucumber, Forest Pansy Redbud (a tree), flame azalea (shrub), stinking hellebore, etc.
- Growing data (problems, companions, tips, guide) is actually solid — 100/100 for those fields
- Image strategy: iNaturalist by scientific name (not Unsplash — wrong subjects returned)
- **Blocked on: Scott's approved plant list.** He is assembling it in a separate app. Do not re-run data collection until list is locked and approved.
- gift-tracker backlog header fix committed locally — no GitHub remote configured for that repo

---

## Han Solo — V1 Build in Progress

Session 3 complete. Session 4 deferred until v1 core is running.

**V1 core build sequence:**
- [x] Letta on Render — `han-solo-letta.onrender.com`, v0.16.7, PostgreSQL + pgvector (2026-05-12)
- [x] Han Solo MCP server — 15 tools live at `han-solo-mcp.onrender.com/mcp`, wired into Claude Code (2026-05-12)
- [ ] **Phase 3 — Seed Ren agent memory** ← next
  - [ ] Write `always_loaded_core` — framework context, working norms, relationship essentials
  - [ ] Write initial `pending_thoughts` — carry-forward from ren-memory
  - [ ] Write first portrait blocks — scott_portrait_forming, ren_portrait_forming
  - [ ] Write a few seed signals — relational and texture from sessions so far
- [ ] Chat UI — Scott and Ted in same room, Ren present, identity markers, light/dark mode
- [ ] Ren's memory seeded — ren-memory + MemPalace migrated, notes layer seeded
- [ ] Session brief — nightly sleeptime job running, brief waiting at session open
- [ ] Private messaging — @mention (quick) and private thread (sustained)
- [ ] Memory state panel — collapsible side panel, three sections

**Pending before notes layer schema:**
- [ ] Ted conversation — his standalone notes product idea. Design notes layer foundation after that conversation.

**Session 4 — Collaboration Model — deferred until v1 is live**
- Ownership model, visibility settings UI, key management UI, full nav

**Deployment reference:** `~/Developer/han-solo/DEPLOYMENT.md` — 14 challenges logged. Read before touching the stack.
