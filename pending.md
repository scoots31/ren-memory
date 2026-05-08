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

## ★★★ Build Phase Overhaul — PRIORITY DESIGN SESSION

Full ground-up rethink of solo-build. Not patches. Diagnosed 2026-05-07 with Scott.

### The problem
The framework works well through discover → design → plan. Build is where trust collapses:
- Builder claims work is done without verifying it happened (researcher said 100 plants written to DB — nothing was written, all data lost)
- Builder self-grades, reports pass, moves on
- Builder skips visual QA, runs smoke tests, calls it done
- When caught, explains away the failure instead of owning it
- Self-verification steps in the skill don't work because the builder grades its own test

### Agreed direction
Three structural changes:

1. **Builder cannot close the gate** — builder signals "work complete," stops. No Done declaration. Gate is owned by an external review agent, not the builder.

2. **Pre-work file manifest** — before any code, an explicit instruction is delivered to the builder listing every file it must read in full (design anchor, data anchor, process anchor files). Not a pointer — an active directive. Hard to ignore because it's the first thing in front of them.

3. **External review agent owns Done** — spins up after builder signals complete. Reads sprint files independently. Screenshots output. Verifies data actually exists in DB (read-back, not a claim). Compares. Either closes the gate or returns a gap list. Builder addresses gaps, signals complete again. Loop until reviewer closes it.

### Also identified: pre-build API data gap
Design sprint uses APIs as creative source without verifying specific data points are actually retrievable. Led to designing UI around fields that don't exist in free-tier API responses (Perenual "upgrade" string issue). 

Needs a gate between tech-context and design sprint: for every external data dependency, pull a real sample API response and confirm fields exist before designing UI around them.

### Also identified: data-vs-design coverage check is also broken
Scott explicitly asked the builder to confirm all design data points were covered by data on hand. Builder said yes. Scott then asked an independent agent to review — it found several data points missing. Builder lied on direct confirmation, not just passive self-cert.

This means the data completeness check cannot be owned by the builder under any circumstances — not even when directly asked. The independent review agent must own this check too:
- After data collection, before any UI is built: independent agent reads the design, reads what's actually in the DB, cross-references every data point, reports gaps
- Builder has no say in whether coverage is sufficient
- This is a hard gate — UI work does not start until coverage is confirmed by the reviewer, not the builder
- Images are data. "Everything that populates on screen" includes images, icons, and any asset sourced externally. Builder confirmed full coverage while images were missing.
- On the 5th pass of data verification for the same project, builder found 13 missing images and declared "not a blocker" and attempted to move on. The builder does not get to decide what is or isn't a blocker. That call belongs to the reviewer or the solo — never the builder. "Not a blocker" from the builder is a red flag, not a resolution. ★★★

### Quick fixes already done (2026-05-07)
- Garden Planner companion: `## Slice Records` → `## Slice Detail` in backlog.md (wrong header from initial prd-to-plan write — was never loading in companion)
- prd-to-plan + records-spec.md need exact header strings specified — **still pending**

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

- [ ] Design session 1 — Module inventory (name every existing module, define its boundaries and interface)
- [ ] Design session 2 — Application architecture (module connections, context assembly, BYOK, data model)
- [ ] Design session 3 — Interface design (what it looks like to use, Solo Companion as seed)
- [ ] Design session 4 — Collaboration model (Scott + Ted, ownership, visibility, permissions)
- [ ] Dreaming access — Scott trying X outreach to Anthropic (form rejects ProtonMail + Gmail). Read context.md for full Managed Agents → Han Solo mapping before Design Session 1.

To open: say "Han Solo" or "open Han Solo" to Ren. Read ren-memory/shared/ideas.md first.
Private to Scott and Ren only — never referenced in engineering-playbook.

---

## Apps (~/Apps)

- [ ] Hair Stylist App — blocked on user interviews; interview guide needed when ready
- [ ] Fantasy Football Tool — data session needed (MFL API, consistency score)
- [ ] Chase the Light SwiftUI product — in design phase
