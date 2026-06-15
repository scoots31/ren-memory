# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-06-15
**scan_run:** 2026-06-15
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-06-15 scan

Four dated entries since last scan (2026-06-01): June 2 (advisor + refusal billing), June 5 (Opus 4.1 deprecation), June 9 (the Claude Fable 5 / Mythos 5 launch — a large multi-item release), and June 10 (an AWS IAM endpoint).

### Act

- **Claude Fable 5 launched (2026-06-09)** — New most-capable *widely released* model (`claude-fable-5`), 1M context by default, 128k max output, always-on adaptive thinking. Same impact as the Opus 4.8 launch last month, but Fable 5 now sits above it as the recommended frontier model — so it affects every place the framework names or assumes a model: build-model recommendations, the multiagent coordinator reference (`claude-opus-4-7` in curator notes), and Han Solo's `ren-v1` agent runtime / Letta config. Ren should reassess the default build-model recommendation and run a model-reference sweep across the skills and Han Solo config. Note: the Opus 4.8 sweep flagged on 2026-06-01 may still be pending — fold both into one pass and land on Fable 5 as the target.

### Watch

- **Managed Agents — scheduled deployments (2026-06-09)** — Sessions can now run on a cron schedule without managing your own scheduler. Directly relevant to Han Solo's nightly session-brief assembly job, currently a homegrown scheduled task. Scott's call on whether Han Solo's continuity scheduler moves onto this — infra decision, not a framework skill change.
- **Managed Agents — vault environment-variable credentials (2026-06-09)** — Secrets can now be injected into the agent sandbox as env vars for CLIs/SDKs. Relevant to Han Solo's BYOK key model and credential handling (own-key / sponsored-key modes). Scott's call on Han Solo's secret-handling architecture.
- **Fable 5 `fallbacks` parameter for refused requests (2026-06-09)** — Opt-in beta parameter re-runs a refused request on another model, billed at the fallback rate. A build-reliability lever for Han Solo's API / validation-wrapper layer if Fable 5 refusals interrupt a build session. Scott's call on whether Han Solo's API layer adopts it.
- **Fable 5 requires 30-day data retention, no zero-data-retention (2026-06-09)** — Fable 5 is not available under ZDR. If Han Solo's NDA-bound / regulated client tier ever requires zero data retention, Fable 5 is off the table for that tier and an older model must be pinned. Scott's call on the Han Solo commercial roadmap, not a framework change.

### Skip

- **`GET /v1/environments/{id}/work` on Claude Platform on AWS (2026-06-10)** — AWS IAM-scoped sandbox endpoint; no framework or Han Solo consumer on the current Render stack.
- **Claude Opus 4.1 deprecation, retirement 2026-08-05 (2026-06-05)** — Neither the framework nor Han Solo references 4.1 (coordinator is `claude-opus-4-7`); housekeeping.
- **Advisor tool `max_tokens` parameter (2026-06-02)** — No framework skill uses the advisor tool; latency/cost optimization detail.
- **No billing for empty `stop_reason: "refusal"` on the Claude API (2026-06-02)** — API billing behavior; no skill consumes it.
- **Fable 5 tokenizer — ~30% more tokens vs pre-4.7 (2026-06-09)** — Same tokenizer as Opus 4.7/4.8, already absorbed; informational.
- **Fable 5 `reasoning_extraction` refusal category (2026-06-09)** — Application-level refusal routing; no framework consumer.
- **Fable 5 adaptive-thinking-only — no `disabled`, no manual budgets, no prefill (2026-06-09)** — Migration constraint; nothing in the framework sets these.
- **Fable 5 `thinking.display` defaults to `"omitted"` (2026-06-09)** — Same as Opus 4.8; no behavioral impact on any skill.
- **`session_thread_id` added to `session.thread_*` webhook events (2026-06-09)** — Minor enhancement to the already-flagged webhooks; no new framework wiring.

---

## Previous Scans

- **2026-06-01** — framework v2.7.0 — 2 Act / 3 Watch / 10 Skip — Opus 4.8 launch (model-reference sweep across skills + Han Solo config) + Claude Code Workflows research preview (overlaps the framework's own sub-agent orchestration — assess replace/augment/coexist).
- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
