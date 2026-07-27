# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-07-24
**scan_run:** 2026-07-27
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-07-27 scan

Three dated entries in the window after 2026-07-15: July 17, July 22, July 24. The July 24 entry is a major model launch — Claude Opus 5 — and it also lands the Opus 4.7 fast-mode removal that the 2026-06-29 scan flagged as a future deadline. Per the standing correction, `last_reviewed` is stamped to the newest assessed item (2026-07-24), not the run date.

### Act

- **Claude Opus 5 launched — `claude-opus-5` (2026-07-24)** — Step-change over Opus 4.8: 1M token context as both default and maximum, 128k max output, thinking on by default, same $5/$25 per MTok pricing; this forces the model-reference sweep that has been pending since the Fable 5 and Opus 4.8 launches — every skill that names a model, plus Han Solo's coordinator config, needs to be evaluated against Opus 5 as the new default coordinator/build model.
- **Fast mode removed for Claude Opus 4.7 (2026-07-24)** — The deprecation flagged in the 2026-06-29 scan has landed: `claude-opus-4-7` with `speed: "fast"` now returns an error with no fallback to standard speed, and ren-memory documents `claude-opus-4-7` as the coordinator model reference in two places (context.md:140, han-solo.md:82) — any config pinning fast mode on that model is now hard-broken and must be checked as the first step of the Opus 5 sweep.
- **Thinking-disable restriction and the effort ladder on Opus 5 (2026-07-24)** — Breaking change from Opus 4.8: `thinking: {"type": "disabled"}` returns a 400 at effort `xhigh` or `max`, and effort (`low` through `max`) is now the primary steering control — any skill or Han Solo agent config that disables thinking or sets an effort level needs a compatibility pass as part of the same sweep.

### Watch

- **Server-side fallback gains a `"default"` mode (2026-07-24)** — The `fallbacks` parameter can now apply Anthropic's recommended fallback models by refusal category behind the `server-side-fallback-2026-07-01` beta header; Han Solo calls the Claude API directly from Render (han-solo.md:558), so this is a real resilience option — Scott's call whether to run a beta header in a production path.
- **Legacy Workbench sunset, access ends 2026-08-17 (2026-07-17)** — Saved prompts, variables, and evals are not carried over to the new Workbench and must be exported before the cutoff; this is not a framework change, but if Scott has anything saved there it is a dated decision with three weeks left on the clock.

### Skip

- **Experimental prompt tools APIs retired 2026-08-17 (2026-07-17)** — `/v1/experimental/generate_prompt`, `improve_prompt`, and `templatize_prompt` will error after removal; no framework skill or Han Solo path calls these endpoints, so nothing to migrate.
- **Claude Managed Agents — effort in agent model config, environment and memory-store webhooks, `initial_events` session seeding, optional `version` on update, thread-level event deltas (2026-07-22, five items)** — All five are Managed Agents runtime plumbing; Han Solo runs on Letta, not the Managed Agents runtime, so there is no consumer — same rationale as the 2026-07-13 and 2026-07-06 scans.
- **Mid-conversation tool changes, beta (2026-07-24)** — Adding or removing tools between turns while preserving the prompt cache, behind `mid-conversation-tool-changes-2026-07-01`; Han Solo's tool set is defined per-agent in Letta rather than swapped mid-conversation, and no other consumer was identified — no skill change.

---

## Previous Scans

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
