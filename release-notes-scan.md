# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-07-10
**scan_run:** 2026-07-13
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-07-13 scan

Four new items in the July 2–10 window. All Skip — no framework or Han Solo consumer this scan. The dominant thread is Managed Agents memory-store plumbing (the `agent-memory-2026-07-22` beta header), which stays inert for us because Han Solo runs on Letta rather than the Managed Agents runtime. `last_reviewed` is stamped to the newest assessed item (2026-07-10), not the run date, per the standing correction, so any item published July 11–13 stays "new" for the next scan.

### Act

None this week.

### Watch

None this week.

### Skip

- **CMEK content preservation docs expanded — reason codes and event payload (2026-07-10)** — Enterprise CMEK / Access Transparency documentation change (`policy_violation_investigation`, `csae_report` reason codes); Han Solo manages its own data on Render and is not on CMEK, so no framework or Han Solo consumer.
- **API key expiration setting in the Console (2026-07-08)** — Optional Console convenience to set an expiry when creating an API/Admin key; existing keys are unaffected and the framework's BYOK model doesn't provision Console keys programmatically, so no skill change — worth a mental note only that a future expiring sponsored key could silently break a deployed agent.
- **`agent-memory-2026-07-22` beta header changes memory-store listing behavior (2026-07-02)** — Stable server-defined ordering, restricted `depth`, whole-segment `path_prefix` on Managed Agents `GET /memory_stores/{id}/memories`; Han Solo runs on Letta, not the Managed Agents runtime, so there is no memory-store consumer to migrate.
- **SDKs now send `agent-memory-2026-07-22` on memory-store calls (2026-07-02)** — Python/TS/Go/Java/Ruby/PHP/C#/CLI SDK version bumps swapping the beta header on memory-store endpoints; SDK maintenance with no framework consumer for the same reason as above.

---

## Previous Scans

- **2026-07-06** — framework v2.7.0 — 1 Act / 3 Watch / 4 Skip — Claude Sonnet 5 launch (`claude-sonnet-5`, 1M context, $2/$10 intro): pending model-reference sweep must evaluate it as default build model + Han Solo coordinator, plus three breaking behavior changes (manual extended thinking → 400, non-default sampling params → 400, adaptive thinking default-on) and a new tokenizer producing ~30% more tokens that reshapes the Context Assembly ceiling math; Fable 5/Mythos 5 access restored and three Managed Agents items (webhooks lifecycle, per-session config override, fast-mode removal) as Watch/Skip.
- **2026-06-29** — framework v2.7.0 — 1 Act / 0 Watch / 2 Skip — Opus 4.7 fast-mode deprecation (errors July 24; opus-4-7 is Han Solo's documented coordinator — fold fast-mode pin check into the pending Opus 4.8 sweep) + rate-limit tier consolidation and code-execution `_20260120` both Skip; established that `last_reviewed` tracks the newest assessed item.
- **2026-06-22** — framework v2.7.0 — 0 Act / 0 Watch / 3 Skip — no items in the June 16–22 window; retroactively assessed the June 11 (×2) and June 15 entries (all Skip), and established the rule that `last_reviewed` tracks the newest assessed item, not the run date.
- **2026-06-15** — framework v2.7.0 — 1 Act / 4 Watch / 9 Skip — Claude Fable 5 launch (model-reference sweep across skills + Han Solo config, land on Fable 5 as default build model, fold in pending Opus 4.8 sweep) + Managed Agents scheduled deployments / vault env-var credentials / Fable 5 fallbacks + ZDR constraint flagged for Han Solo.
- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
