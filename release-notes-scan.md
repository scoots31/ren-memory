# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-07-01
**scan_run:** 2026-07-06
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-07-06 scan

Eight new items across the June 29 – July 1 window (the June 26 rate-limit entry was assessed last scan). The dominant theme is a model-landscape shift: Claude Sonnet 5 launched, Fable 5 / Mythos 5 access was restored, and Opus 4.6 fast mode was removed — all of which feed the still-pending model-reference sweep. `last_reviewed` is stamped to the newest assessed item (2026-07-01), not the run date, per the standing correction, so any item published July 2–6 stays "new" for the next scan.

### Act

- **Claude Sonnet 5 launched — `claude-sonnet-5` (2026-06-30)** — Highest-priority framework input this scan: the pending model-reference sweep must now evaluate Sonnet 5 (1M context, 128k output, introductory $2/$10 through Aug 31) as a candidate default build model and Han Solo coordinator, and three breaking behavior changes need a config check across framework skills and Han Solo/Letta — manual extended thinking (`thinking: {budget_tokens}`) now returns 400, non-default sampling params (`temperature`/`top_p`/`top_k`) now return 400, and adaptive thinking is default-on; also the new tokenizer produces ~30% more tokens for the same text, which affects the context-ceiling math in Context Assembly.

### Watch

- **Claude Fable 5 / Mythos 5 access restored (2026-07-01)** — The pending sweep was leaning toward Fable 5 as the framework's default build model; with Fable 5 back and Sonnet 5 now available, this is Scott's call on which model becomes the recommended default.
- **Managed Agents webhooks now cover deployment lifecycle (2026-06-30)** — Extends the event-driven path already flagged for Solo Companion (session events) to agent-version-published, deployment-paused, and failed-scheduled-run events without polling; Scott's call on whether Han Solo adopts the Managed Agents runtime to use it (Han Solo currently runs on Letta).
- **Managed Agents per-session config override — `agent_with_overrides` (2026-06-30)** — Override model, system prompt, tools, MCP servers, or skills for a single session without changing the agent; could shape Han Solo's multiagent design (one agent, per-session swaps), a direction decision rather than a framework skill change.

### Skip

- **Fast mode removed for Claude Opus 4.6 (2026-06-29)** — Degrades gracefully (runs at standard speed and pricing, no error, `usage.speed` reports actual); the pending fast-mode/model-reference sweep already covers Opus 4.7, which *does* error on July 24, so no new action beyond finishing that sweep.
- **Managed Agents event deltas (2026-06-30)** — Streaming preview of agent message text before the complete event arrives; no framework or Han Solo consumer, since Han Solo runs on Letta rather than the Managed Agents runtime.
- **Managed Agents backward pagination for listing sessions (2026-06-30)** — `prev_page` cursor on `GET /v1/sessions`; API convenience with no framework consumer.
- **Managed Agents vault `injection_location` for env-var credentials (2026-06-30)** — Controls whether a credential is injected into outbound headers, body, or both; Han Solo manages its own secrets on Render, so no consumer.

---

## Previous Scans

- **2026-06-29** — framework v2.7.0 — 1 Act / 0 Watch / 2 Skip — Opus 4.7 fast-mode deprecation (errors July 24; opus-4-7 is Han Solo's documented coordinator — fold fast-mode pin check into the pending Opus 4.8 sweep) + rate-limit tier consolidation and code-execution `_20260120` both Skip; established that `last_reviewed` tracks the newest assessed item.
- **2026-06-22** — framework v2.7.0 — 0 Act / 0 Watch / 3 Skip — no items in the June 16–22 window; retroactively assessed the June 11 (×2) and June 15 entries (all Skip), and established the rule that `last_reviewed` tracks the newest assessed item, not the run date.
- **2026-06-15** — framework v2.7.0 — 1 Act / 4 Watch / 9 Skip — Claude Fable 5 launch (model-reference sweep across skills + Han Solo config, land on Fable 5 as default build model, fold in pending Opus 4.8 sweep) + Managed Agents scheduled deployments / vault env-var credentials / Fable 5 fallbacks + ZDR constraint flagged for Han Solo.
- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
