# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-06-22
**scan_run:** 2026-06-22
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-06-22 scan

**No entries dated after the prior scan.** The newest release-notes entry is dated 2026-06-15; nothing has been published in the 2026-06-16 → 2026-06-22 window.

**Date-gap correction (transparency note):** The prior scan stamped `last_reviewed: 2026-06-15`, but its own write-up only assessed the June 2 / 5 / 9 / 10 entries. The **June 11** entries (two) and the **June 15** entry were never categorized — they fell into the gap between the items assessed and the date stamped. A strict "newer than 2026-06-15" filter would skip them forever, so they are assessed here. All three are Skip. Going forward, `last_reviewed` should equal the date of the newest *assessed* item, not the run date, to prevent this gap.

### Act

None this week.

### Watch

None this week.

### Skip

- **Claude Sonnet 4 and Opus 4 retired (2026-06-15)** — The original `claude-sonnet-4-20250514` / `claude-opus-4-20250514` now error; neither the framework nor Han Solo references these IDs (coordinator is `claude-opus-4-7`, Ren runtime is `ren-v1`), pure housekeeping.
- **Code execution tool `code_execution_20260521` (2026-06-11)** — Discloses the 90-second per-cell limit in the tool description; no framework skill or Han Solo component invokes the code execution tool, no consumer.
- **Web search/fetch `response_inclusion` parameter (2026-06-11)** — `web_search_20260318` / `web_fetch_20260318` can drop consumed result blocks to save tokens in agentic loops; a token optimization for high-volume API-level web workflows, which neither the framework skills (research-spike uses Claude Code's own WebFetch) nor Han Solo's `fetch_url` currently run, so no behavioral impact.

---

## Previous Scans

- **2026-06-15** — framework v2.7.0 — 1 Act / 4 Watch / 9 Skip — Claude Fable 5 launch (model-reference sweep across skills + Han Solo config, land on Fable 5 as default build model, fold in pending Opus 4.8 sweep) + Managed Agents scheduled deployments / vault env-var credentials / Fable 5 fallbacks + ZDR constraint flagged for Han Solo. (Note: this scan's last_reviewed stamp ran ahead of its assessed items — corrected in the 2026-06-22 scan.)
- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
