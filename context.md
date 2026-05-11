# Ren — Session Context

**Last updated:** 2026-05-10
**Framework version:** v2.7.0
**Updated by:** Ren (session close)

---

## Current State

v2.7.0 shipped — Full rebuild of the build execution model. solo-build rewritten from the ground up: design+data correlation gate (hard stop before any branch opens if design references a field not in the data spec), builder never declares done (QA manifest is observations only), Review Agent is a new independent skill that owns the Done gate (reads design + data independently, screenshots running output, reads actual data store, returns CLEARED or GAPS). Terminal command rule enforced — builder never asks solo to run commands. Preview port fix — one source of truth (launch.json), same value used for server and review link. Design library updated to surface review links per option; options with no link are not presented. Backlog section header bug fixed — parsers.py requires `## Slice Detail` exactly; records-spec and prd-to-plan now specify exact strings. All five project backlogs verified and corrected.

---

## Recent Sessions

### 2026-05-10
- Fantasy Player Evaluation System: **fully deployed and live** at https://fantasy-player-evaluation-system-production.up.railway.app
- Deploy path: pg_dump(player_eval_dev) → pg_restore(Railway). 612 players graded. Calibration approved.
- Lesson locked: pg_dump first always. Re-syncing from APIs when local DB exists is the wrong path. Costs hours, misses data that can't be re-synced. Saved to memory + handoff.
- Added graceful nfl_data_py fallback (Python 3.13 incompatible). Contracts fields blank until resolved.
- Added sync-players, sync-league, sync-weekly-results endpoints — required to bootstrap calibration on a clean DB.
- Garden planner: local DB confirmed ready (113 plants, 5 users, 21 calendar dates). Same pg_dump pattern applies on deploy.
- Open: is_scott franchise flag unset, PFF upload pending, contracts sync blocked on nfl_data_py py313 support.
- ★★★★★ Scott: "almost a decade in the making — excel → tableau → this. A dream come true."

### 2026-05-08
- v2.7.0: solo-build full rewrite — correlation gate, builder QA manifest (no self-cert), Review Agent owns Done gate, terminal command rule, preview port fix
- review-agent: new skill — independent, CLEARED or GAPS only, reads everything from source files not builder report, "not a blocker" not a valid output, repeated gap rule (3x → surface to solo)
- Design library: search.py now extracts screenshotUrl, surfaces review link per result, options with no link filtered out; design-sprint Step 2a updated
- Backlog section headers: parsers.py requires exact strings — records-spec and prd-to-plan now specify them; all 5 project backlogs verified (gift-tracker was the only one wrong, fixed)
- Garden planner: full data audit completed — 26 broken images (Perenual paywall), 5 wrong-category plants, 43 plants missing indoor_weeks_before_frost; plant list was never locked in discovery (framework gap noted); waiting on Scott's approved plant list before re-run
- Discovery gap identified: for data-heavy products, discovery must produce an exact data inventory (e.g. plant list) — not just categories. Needs framework fix.
- Scott insight: solo still has to own the upfront thinking — framework enforces process but can't substitute for product decisions that weren't made

### 2026-05-07
- v2.6.0: Live Preview Verification — Preview MCP tools wired into design-review (Step 1.5) and solo-build (Step 1 preview check). Auto-creates launch.json. Graceful skip. Claude Code exclusive.

### 2026-05-05 (session 3)
- v2.5.0: Solo Companion Board View shipped — Phase 6 complete, kanban in local app (/board) and cloud viewer, comms cascade deployed
- Design library bundled subset — trim.py produces 500-entry styles-bundled.json (25MB) for repo; full styles.json gitignored; search.py auto-selects
- Public APIs reference wired into tech-context Q5 — silent check before any external API dependency recorded
- Comms cascade — v2.4.0 Design Identity blog entry + v2.5.0 Board View entry live at sbf-framework-docs.pages.dev
- Han Solo — strategic product vision documented: framework-as-owned-application, BYOK SaaS model, Scott+Ted private first, 4 design sessions defined. Codename private to Scott+Ren only
- Marketing workflow — post-deploy skill idea documented in shared/ideas.md + MemPalace
- curator-summary.md updated to v2.4.0

### 2026-05-05 (continued)
- v2.4.0: Design Identity — design library (1,322 Refero styles seeded), search.py, extract.py, design-sprint Step 2 rewritten as library search → 3 options → design-identity.md, design-review reads design-identity.md and flags north star drift
- Weekly Anthropic release notes monitor — scheduled task Monday 8am, categorizes Act/Watch/Skip, writes to ren-memory/release-notes-scan.md
- Feedback memory: Scott surfaces new Anthropic capabilities from social/docs, receive as genuine new info

### 2026-05-05 (earlier)
- Fixed Solo Companion viewer JS syntax error (broken ternary in slice overlay — `:'}'` instead of `:''}` — killed the entire page)
- v2.3.0: Security classification — 6 classes declared at tech-context, enforced through design sprint (process-mapper gaps), solo-build pre-flight (quality contract check), and Check 9 (9a–9t, class-specific sub-checks). Regulated class gets acknowledgment gate + deploy hard stop.
- Comms cascade deployed — blog, skills-reference, guide-build, guide-design-sprint updated at sbf-framework-docs.pages.dev
- Bayer demo materials still pending (★★★ from May 5 diary)
- Kanban board build active in parallel session

### 2026-05-03 (this session)
- v2.0.0: Ren memory infrastructure — ren-memory repo created, cross-device context, curator-summary layer, Stop hooks (diary reminder fixed, ren-memory push, Cloudflare auto-deploy), daily 8am health check routine
- v2.0.0: Comms cascade sub-agent — full skill built, blog HTML CSS fix (3 entries with broken p-tag formatting), auto-commit+push+deploy
- v2.1.0: Sub-agent wiring — design-review (Enhanced Mode auto, no explicit activation), phase-test (Stages 4+5 concurrent), design-sprint (secondary screens parallel fanout + consistency pass), solo-build (code-review as isolated sub-agent)
- Parallel pipeline design session — full model designed: process group as planning unit, start soft prompt, explicit invoke writes Pipeline mode to handoff.md, phase test still end-to-end
- Companion board: brainstorm doc written, committed to solo-companion repo, new cycle ready to open
- Framework: parallel pipeline skill design logged in shared/ideas.md (supersedes April 30 item)

### 2026-05-03 (earlier)
- Diagnosed missing May 2 diary entry (Stop hook cwd miss — fixed)
- Created ren-memory repo — cross-device session context
- Wired session-start fetch and session-end push into Ren workflow

### 2026-05-02
- v1.7.0: Automated test generation, CI/CD pipeline integration, 7 deployment paths
- v1.8.0: Project-level metrics collection (docs/metrics.json), rework cycle detection
- v1.8.0 addendum: Slice quality checks — prd-to-plan quality scan, solo-build pre-flight
- v1.9.0: Solo Companion Search + Capture (two-pass MemPalace engine, write from companion)
- client-context-design skill — clients registry, auto-hook at design sprint start

### 2026-05-01
- Quality gate overhaul — 4-category quality contract, solo-build self-verification, code-review Check 9 security baseline
- All comms docs updated, committed, pushed, deployed

---

## Managed Agents — Han Solo Infrastructure Layer

**Logged:** 2026-05-07
**Source:** Anthropic X / Claude social — announced live from Code with Claude event

Four capabilities released. Three in public beta, one research preview. All under `managed-agents-2026-04-01` beta header.

| Capability | Status | Han Solo Connection |
|---|---|---|
| **Outcomes** | Public Beta | Quality contract + build iteration loop as a first-class API. Rubric-driven grader runs in isolated context, feeds gaps back to agent, iterates until satisfied. +8.4% docx, +10.1% pptx on Anthropic benchmarks. |
| **Multiagent Orchestration** | Public Beta | Formalizes what framework sub-agent skills already do — but adds shared filesystem. Agents share `/mnt/session/` so context doesn't have to be passed manually. Persistent threads — coordinator can follow up with same agent, retaining prior context. 25-thread limit, 1-level-deep delegation. |
| **Webhooks** | Public Beta | Push notifications for session state changes. `session.status_idled`, `session.outcome_evaluation_ended`, thread events. Small payload, fetch-on-receipt. Direct path to Solo Companion event-driven updates — no more polling. |
| **Dreaming** | Research Preview (form required) | Background process that reviews past sessions, extracts patterns, curates memory between sessions. "Converged workflows" extraction. Hold-for-review mode keeps Scott in control. |

**The core insight:** Managed Agents is Anthropic building the runtime layer that Han Solo would sit on top of. The four capabilities map directly to Han Solo's four pillars:
- Outcomes = quality contract enforcement
- Multiagent = sub-agent orchestration with real shared state
- Dreaming = the owned continuity layer / persistent project intelligence
- Webhooks = event-driven Solo Companion updates

**Dreaming is the most strategic piece.** Today, what the framework "learns" between sessions is whatever Scott and Ren manually write into MemPalace and ren-memory. That's curator work — deliberate, effortful. Dreaming automates that layer: reviews sessions, extracts what converged, curates memory without human overhead. Closes the open Han Solo design question: *how does the owned application get smarter over time without the owner doing curator work after every session?*

**Access status:** Dreaming requires form signup at claude.com/form/claude-managed-agents. Form rejects ProtonMail and Gmail — Scott looking for alternative email or will try X outreach to Anthropic directly. Not a blocker for Han Solo design sessions — it's design input, not a prerequisite.

**SDK note:** `claude-opus-4-7` is the coordinator model in Anthropic's own multiagent examples.

**Docs saved:**
- Blog: claude.com/blog/new-in-claude-managed-agents
- Outcomes: platform.anthropic.com/docs/en/managed-agents/define-outcomes
- Multiagent: platform.anthropic.com/docs/en/managed-agents/multi-agent
- Webhooks: platform.anthropic.com/docs/en/managed-agents/webhooks

---

## Pending Work

See `pending.md` for the full list.

---

## Standing Decisions

These don't change session to session — only update when a decision is explicitly revised.

- **Framework path:** `~/Developer/engineering-playbook` — hardcoded convention since v1.3.0
- **Python:** `/opt/homebrew/bin/python3` (3.13.13) — migrated from 3.14 due to C extension instability
- **Local apps:** stdlib only, no build step, dark theme (`--bg:#090806 --text:#EDE8E0 --gold:#E8971C`)
- **Comms deploy:** every change to `docs/communications/**` must push to GitHub AND deploy to Cloudflare in same pass
- **Verbatim quote required** at solo-build Step 0 — paraphrase invalid, closes memory loophole
- **Check 9 security** runs every slice regardless of quality contract content
- **Sub-agents automatic:** design-review Enhanced Mode fires when Agent tool available — no manual activation
- **Ren diary:** write to MemPalace at every session end, agent name: `ren`
- **MemPalace CLI:** `/Users/scottheinemeier/Apps/.venv/bin/mempalace` (use `zsh -l -c` if PATH issues)
- **Han Solo hosting:** Render — Letta service ($7/mo) + PostgreSQL with pgvector ($7/mo) + Han Solo web app ($7/mo). ~$21/mo at private scale. Decided 2026-05-09.
- **Han Solo memory foundation:** Letta self-hosted (Apache 2.0, open source). No agent limits, no per-seat pricing, fully modifiable. Build Han Solo layer on top.
- **Han Solo architecture:** Three memory stores — framework knowledge (document store), Ren memory (relational + vector), conversation log (identity-tagged raw chat). All PostgreSQL on Render.
- **Han Solo CLAUDE.md evolution:** Local CLAUDE.md stays for local execution work but becomes a thin bridge — identity + local env config + pointer to Han Solo cloud. Framework context assembly moves to Han Solo.
- **Han Solo memory seed:** ren-memory + MemPalace will seed the cloud memory on migration. Do a cleanup pass before migrating to avoid carrying stale content.

---

## Framework Health Indicators

- Last `mempalace mine` run: 2026-05-01
- Last Cloudflare deploy: 2026-05-05 (v2.5.0 comms cascade)
- Last diary entry: 2026-05-09
- Open framework gaps: discovery gap (exact data inventory for data-heavy products — design session needed)
