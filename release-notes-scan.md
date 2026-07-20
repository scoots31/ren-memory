# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-07-15
**scan_run:** 2026-07-20
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-07-20 scan

Two new items in the July 11–20 window (July 14, July 15), plus one same-date (July 10) item — Dreams model support — that the prior scan (run 2026-07-13) did not assess and which is surfaced here rather than omitted. Per the standing correction, `last_reviewed` is stamped to the newest assessed item (2026-07-15), not the run date, so anything published July 16–20 stays "new" for the next scan.

### Act

None this week.

### Watch

- **Dreams (research preview) now supports Claude Fable 5 and Claude Sonnet 5 (2026-07-10)** — Dreams is Anthropic's native between-session memory curation; context.md flags Dreaming as "the most strategic piece" of the owned-continuity layer, but Scott is form-blocked (ProtonMail/Gmail rejected) and Han Solo already solves this via Letta sleeptime agents — Scott's call whether to pursue Dreams access now that the default build model (Fable 5) is supported, or stay on the Letta-native path. Same-date-as-prior-scan item, not assessed in the 2026-07-13 run.

### Skip

- **Mid-conversation system messages extended to Fable 5 and Mythos 5 (2026-07-15)** — Availability correction adding an existing Opus 4.8 feature (shipped 2026-05-28, already Act-swept in the Opus 4.8 launch) to Fable 5/Mythos 5 with no beta header; the framework injects context via the Claude Code UserPromptSubmit hook (user-prompt content, not Messages-API system messages) and Han Solo runs on Letta, so there is no consumer that can adopt it — no skill change.
- **Claude Enterprise Admin API user management, beta (2026-07-14)** — Org member/group/role management for Claude Enterprise (claude.ai) organizations; Scott is a solo builder not on Claude Enterprise, so there is no framework or Han Solo consumer.

---

## Previous Scans

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
