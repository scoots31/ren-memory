# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-08-07
**scan_run:** 2026-08-10
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-08-10 scan

Three dated entries published since the last assessed item (2026-07-24): August 1, August 5, and August 7. Seven items total. `last_reviewed` advances to 2026-08-07, the newest assessed item.

Every August 7 item is Claude Managed Agents surface. Han Solo runs on Letta, not the Managed Agents runtime, so none of them has a framework consumer today — but two of them are directionally relevant enough to Han Solo's own design that they belong in front of Scott rather than buried in Skip.

### Act

None this week. The Opus 5 model-reference sweep raised 2026-07-24 remains the open Act item — see Carry-Forward.

### Watch

- **Managed Agents session advisor (2026-08-07)** — A session's primary thread can now consult a same-or-more-capable model mid-turn for strategic guidance, configured as a `{"type": "advisor"}` entry in the multiagent roster. This is Anthropic formalizing the pattern the framework already runs by hand (review-agent, design-review Enhanced Mode, Recon as advisory-only reviewer). Scott's call: whether Han Solo's coordinator design adopts the advisor shape now, on the assumption a Managed Agents path opens later, or stays with the current spawn-a-reviewer approach on Letta.
- **Managed Agents sessions load skills from a mounted GitHub repo (2026-08-07)** — Any skills in a mounted repository's root `.claude/skills` are auto-discovered at session start. This is a second distribution path for framework skills alongside the current two (Framework Vers1 repo + the `agent_skills` table in the Han Solo DB). Scott's call: whether a repo-mounted path is worth keeping in view, given the standing rule that Framework Vers1 + the DB are the single source and that a third path is exactly the kind of split that caused the engineering-playbook problem.
- **Dreams (research preview) now supports Claude Opus 5 (2026-08-01)** — Third model-support expansion in a month (Fable 5 and Sonnet 5 landed 2026-07-10, raised as Watch on the 07-20 scan). Same unresolved decision as before: Dreams versus the Letta sleeptime path for Han Solo's between-session memory curation. Opus 5 support raises what Dreams could do without changing that the access form was never completed.

### Skip

- **Claude Opus 4.1 retired — `claude-opus-4-1-20250805` now errors (2026-08-05)** — Verified by grep across `ren-memory`, `han-solo`, and `Framework Vers1`: zero references to `claude-opus-4-1` in any file. Nothing breaks. (The same sweep found the model pins that *are* live and still unassessed against Opus 5 — recorded under Carry-Forward.)
- **Inference hooks beta for Claude Enterprise (2026-08-05)** — Routes every governed prompt across claude.ai, Cowork, and Claude Code through an org AI-security server for an allow/deny verdict. Enterprise-tier only; Scott is not on Enterprise and has no org security server.
- **Managed Agents session budgets (2026-08-07)** — Hard spend cap per session, pausing with a `budget_reached` stop reason. Managed Agents runtime only — no equivalent lever exists on the Letta path, so nothing in the framework or Han Solo can consume it.
- **Managed Agents `inference_geo` data residency (2026-08-07)** — Controls which geo runs inference for an agent or session. Managed Agents runtime only; no residency constraint on any current project.

---

## Carry-Forward — dated items still open

- **Opus 5 model-reference sweep (raised 2026-07-24, Act)** — Still the open framework item. This scan's grep produced the concrete target list, all still unassessed against Opus 5:
  - `han-solo/han_solo/runtime/providers/anthropic.py:13` → `claude-opus-4-8`
  - `han-solo/han_solo/handoff.py:39` → `claude-sonnet-4-6`
  - `han-solo/agents/dist/assets/index-Gyez3AXY.js:40` → `claude-fable-5` (built asset — source needs finding, not the bundle)
  - `ren-memory/context.md:140` → `claude-opus-4-7`, documented as the coordinator model. Fast mode for `claude-opus-4-7` was removed 2026-07-24 and now hard-errors, so any config pinning it with `speed: "fast"` is already broken.
  - Plus the thinking-disable compatibility pass: on Opus 5, `thinking: {"type": "disabled"}` at effort `xhigh` or `max` returns a 400.
- **Legacy Workbench sunset — access ends 2026-08-17 (raised 2026-07-17)** — One week left. Saved prompts, variables, and evals do not carry to the new Workbench and must be exported before the cutoff. Still Scott's call, still unresolved. This is the last scan before the deadline.
- **Experimental prompt tools APIs retired 2026-08-17** — Same date, still Skip; no framework skill or Han Solo path calls those endpoints.

---

## Previous Scans

- **2026-08-03** — framework v2.7.0 — 0 Act / 0 Watch / 0 Skip — no new items; newest entry still the 2026-07-24 Opus 5 launch. Confirmed `docs.anthropic.com/en/release-notes/overview` now 301-redirects to the platform URL — one source, not a fetch failure.
- **2026-07-27** — framework v2.7.0 — 3 Act / 2 Watch / 3 Skip — Claude Opus 5 launch (`claude-opus-5`, 1M context default and max, 128k output, thinking on by default, $5/$25) forces the long-pending model-reference sweep; Opus 4.7 fast-mode removal landed and hard-breaks any config pinning it; thinking-disable now 400s at effort `xhigh`/`max`; server-side fallback `"default"` mode and the Workbench sunset as Watch.
- **2026-07-20** — framework v2.7.0 — 0 Act / 1 Watch / 2 Skip — no framework consumer this scan; Dreams research preview gaining Fable 5 / Sonnet 5 support raised as Scott's call against the Letta sleeptime path; mid-conversation system messages on Fable 5/Mythos 5 and the Claude Enterprise Admin API both Skip.
- **2026-07-13** — framework v2.7.0 — 0 Act / 0 Watch / 4 Skip — no framework consumer this scan; dominant thread was Managed Agents memory-store plumbing (`agent-memory-2026-07-22` beta header) which stays inert because Han Solo runs on Letta, not the Managed Agents runtime; CMEK content-preservation docs and Console API-key expiration both Skip.
- **2026-07-06** — framework v2.7.0 — 1 Act / 3 Watch / 4 Skip — Claude Sonnet 5 launch (`claude-sonnet-5`, 1M context, $2/$10 intro): pending model-reference sweep must evaluate it as default build model + Han Solo coordinator, plus three breaking behavior changes (manual extended thinking → 400, non-default sampling params → 400, adaptive thinking default-on) and a new tokenizer producing ~30% more tokens that reshapes the Context Assembly ceiling math; Fable 5/Mythos 5 access restored and three Managed Agents items (webhooks lifecycle, per-session config override, fast-mode removal) as Watch/Skip.
- **2026-06-29** — framework v2.7.0 — 1 Act / 0 Watch / 2 Skip — Opus 4.7 fast-mode deprecation (errors July 24; opus-4-7 is Han Solo's documented coordinator — fold fast-mode pin check into the pending Opus 4.8 sweep) + rate-limit tier consolidation and code-execution `_20260120` both Skip; established that `last_reviewed` tracks the newest assessed item.
- **2026-06-22** — framework v2.7.0 — 0 Act / 0 Watch / 3 Skip — no items in the June 16–22 window; retroactively assessed the June 11 (×2) and June 15 entries (all Skip), and established the rule that `last_reviewed` tracks the newest assessed item, not the run date.
- **2026-06-15** — framework v2.7.0 — 1 Act / 4 Watch / 9 Skip — Claude Fable 5 launch (model-reference sweep across skills + Han Solo config, land on Fable 5 as default build model, fold in pending Opus 4.8 sweep) + Managed Agents scheduled deployments / vault env-var credentials / Fable 5 fallbacks + ZDR constraint flagged for Han Solo.
- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
