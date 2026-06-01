# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-06-01
**scan_run:** 2026-06-01
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-06-01 scan

Two dated entries since last scan (2026-05-25): a large May 28 release built around the Opus 4.8 launch, and a May 29 Managed Agents-on-AWS entry.

### Act

- **Claude Opus 4.8 launched (2026-05-28)** — New most-capable GA model, 1M context by default, 128k max output, adaptive thinking, effort defaulting to `high`. Affects every place the framework names or assumes a model — the multiagent coordinator reference (`claude-opus-4-7` in curator notes), Han Solo's agent runtime, and any build-model recommendation. Ren should propose a model-reference sweep across the skills and Han Solo config at the next session.
- **Claude Code Workflows — research preview (2026-05-28)** — Native primitive for defining and running multi-step agentic plans inside Claude Code. Directly overlaps the framework's own sub-agent orchestration and the in-progress parallel pipeline design — Ren should assess whether Workflows replaces, augments, or sits beside the homegrown phase-chain and sub-agent wiring (design-review Enhanced Mode, phase-test concurrency, design-sprint fanout).

### Watch

- **Mid-conversation system messages (2026-05-28)** — On Opus 4.8, `role: "system"` messages can be sent at non-first positions, preserving prompt-cache hits when instructions change mid-session. No framework skill calls the Messages API directly (Claude Code does), but it's a real lever for Han Solo's context-assembly layer where phase/instruction shifts happen mid-session. Scott's call on whether Han Solo's context assembly adopts it.
- **Claude Code Auto mode expanded + Max-plan fast mode default on 4.8 (2026-05-28)** — Auto mode for long-running tasks is now open to more users, and Max-plan users default to fast mode on Opus 4.8. Changes how Scott's own build sessions run day to day; potentially relevant to the autopilot skill's autonomy assumptions. Scott's call on whether to lean on Auto mode for autonomous builds.
- **Managed Agents on Claude Platform on AWS (2026-05-29)** — Webhooks, multiagent orchestration, and self-hosted sandboxes now available via AWS with IAM auth and a managed access policy. Same theme as last week's self-hosted-sandboxes Watch: the data-privacy path for a regulated/NDA Han Solo client tier. Scott's call on the Han Solo commercial roadmap, not a framework change.

### Skip

- **Refusal categories in `stop_details` (2026-05-28)** — Application-level API routing; no framework skill consumes refusal classes.
- **Effort defaults to `high` on 4.8 (2026-05-28)** — Automatic behavior change; affects build cost/depth but needs no skill edit.
- **Min cacheable prompt length 1,024 tokens on 4.8 (2026-05-28)** — Caching optimization detail, no behavioral impact on any skill.
- **Adaptive thinking on 4.8 (2026-05-28)** — Automatic; reduces wasted thinking tokens, nothing to wire.
- **High-resolution image input on 4.8 (2026-05-28)** — Vision parity with 4.7; no framework consumer.
- **Task budgets / advisor tool / computer use now support 4.8 (2026-05-28)** — Tool-compatibility extensions to the new model; no framework change.
- **Fast mode for 4.8 — research preview, API only (2026-05-28)** — API-side preview, separate from Claude Code's `/fast`; informational.
- **Sampling params return 400 on 4.8 (2026-05-28)** — Same constraint as 4.7; migration detail.
- **Fast mode for Opus 4.6 deprecated (2026-05-28)** — Removal ~30 days out; migrate to 4.7/4.8 fast mode. Housekeeping.
- **claude.ai / Cowork / M365 release notes (2026-05-28)** — Consumer-app updates, outside framework scope.

---

## Previous Scans

- **2026-05-25** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — Managed Agents family (2026-05-19): MCP tunnels research preview + self-hosted sandboxes flagged as the Han Solo private-network / data-privacy path; no Act.
- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
