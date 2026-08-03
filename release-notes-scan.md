# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-07-24
**scan_run:** 2026-08-03
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-08-03 scan

No new items this week. The newest dated entry on the platform release notes page is still **July 24, 2026** (the Claude Opus 5 launch), which is exactly where the last scan stopped — nothing has been published in the ten days since. Per the standing rule, `last_reviewed` stays at 2026-07-24 (the newest assessed item) rather than advancing to the run date.

The fallback URL (`docs.anthropic.com/en/release-notes/overview`) now 301-redirects to `platform.claude.com/docs/en/release-notes/overview` — same content, one source. Noting it so future scans don't treat the redirect as a fetch failure.

### Act

None this week.

### Watch

None this week.

### Skip

None this week.

---

## Carry-Forward — dated items still open

- **Legacy Workbench sunset — access ends 2026-08-17 (raised 2026-07-17)** — two weeks left. Saved prompts, variables, and evals do not carry over to the new Workbench and must be exported before the cutoff. Still Scott's call, still unresolved.
- **Experimental prompt tools APIs retired 2026-08-17** — same date, still assessed as Skip; no framework skill or Han Solo path calls those endpoints.
- **Opus 5 model-reference sweep (raised 2026-07-24, Act)** — still the open framework item: every skill that names a model, plus Han Solo's coordinator config, needs evaluation against Opus 5, including the now-hard-broken `claude-opus-4-7` + `speed: "fast"` pin (context.md:140, han-solo.md:82) and the thinking-disable / effort-ladder compatibility pass.

---

## Previous Scans

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
