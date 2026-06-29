# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-06-26
**scan_run:** 2026-06-29
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-06-29 scan

Three items assessed: the two published after the prior stamp (June 25, June 26), plus the **June 18** `code_execution_20260120` SDK entry, which predates the 2026-06-22 stamp but was never categorized (it fell into the same assessed-vs-stamped gap the prior scan corrected). `last_reviewed` is stamped to the newest *assessed* item (2026-06-26), not the run date, per the prior scan's standing correction — this prevents late-published items in the June 27–29 window from being skipped forever.

### Act

- **Fast mode deprecated for Claude Opus 4.7 (2026-06-25)** — `claude-opus-4-7` with `speed: "fast"` errors after July 24, 2026; opus-4-7 is the documented coordinator model in Han Solo's multiagent examples, so confirm no framework skill or Han Solo/Letta config pins it with fast mode and fold the check into the still-pending Opus 4.8 model-reference sweep before the removal date. (Note: this is the API `speed` parameter, distinct from Claude Code's `/fast` toggle — likely no live consumer, but the deadline makes finishing the sweep time-sensitive.)

### Watch

None this week.

### Skip

- **API rate limits raised, usage tiers consolidated to Start/Build/Scale (2026-06-26)** — Sonnet/Haiku limits now match Opus at every tier; net-positive headroom for high-volume agentic builds (and Han Solo's sponsored-key cost model) with no action required and no org receiving lower limits, so no framework change and no decision for Scott.
- **Code execution tool `code_execution_20260120` in all SDKs (2026-06-18)** — Adds REPL state persistence and is the minimum version for programmatic tool calling; no framework skill or Han Solo component invokes the code execution tool (consistent with the June 11 code-execution skips), so no consumer.

---

## Previous Scans

- **2026-06-22** — framework v2.7.0 — 0 Act / 0 Watch / 3 Skip — no items in the June 16–22 window; retroactively assessed the June 11 (×2) and June 15 entries that the prior scan's date stamp had skipped (all Skip), and established the rule that `last_reviewed` tracks the newest assessed item, not the run date.
- **2026-06-15** — framework v2.7.0 — 1 Act / 4 Watch / 9 Skip — Claude Fable 5 launch (model-reference sweep across skills + Han Solo config, land on Fable 5 as default build model, fold in pending Opus 4.8 sweep) + Managed Agents scheduled deployments / vault env-var credentials / Fable 5 fallbacks + ZDR constraint flagged for Han Solo. (Note: this scan's last_reviewed stamp ran ahead of its assessed items — corrected in the 2026-06-22 scan.)
- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
