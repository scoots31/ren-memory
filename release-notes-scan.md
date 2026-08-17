# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-08-11
**scan_run:** 2026-08-17
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-08-17 scan

Two dated entries published since the last assessed item (2026-08-07): August 10 and August 11. Three items total. `last_reviewed` advances to 2026-08-11, the newest assessed item.

Light week on the platform side. The one item that matters is a pricing decision, not a capability — and the thing that actually needs attention today is a carry-forward deadline, not a new release.

### Act

None this week. The Opus 5 model-reference sweep raised 2026-07-24 remains the open Act item — see Carry-Forward, where this scan's grep expanded the target list.

### Watch

- **Claude Sonnet 5 introductory pricing is now permanent (2026-08-10)** — The scheduled September 1 increase from $2/$10 to $3/$15 per MTok will not happen; $2/$10 is the standard price. This is Scott's call on the still-pending model-reference sweep: the sweep has to pick a default build model, and Sonnet 5 holding at $2/$10 permanently against Opus 5 at $5/$25 makes that a durable 2.5× spread rather than a temporary promotional one. The decision it forces is whether the framework's default build model is Sonnet 5 with Opus 5 reserved for capability-critical slices, or Opus 5 across the board.

### Skip

- **Compliance API returns local Cowork and Claude Code session transcripts (2026-08-11)** — `GET /v1/compliance/apps/sessions/local` and its two sibling endpoints, beta for Claude Enterprise organizations only, requiring a Compliance Access Key with the `read:compliance_user_data` scope. Scott is not on Claude Enterprise and has no Compliance Access Key; no endpoint here is callable from any framework or Han Solo path.
- **`anthropic-workspace-id` response header (2026-08-11)** — Every Claude API response now carries the `wrkspc_`-prefixed ID of the workspace the request's key resolved to. No framework consumer: Han Solo's Anthropic provider (`han-solo/han_solo/runtime/providers/anthropic.py`) reads no response headers for attribution, and there is no multi-workspace setup to disambiguate.

---

## Carry-Forward — dated items still open

- **Legacy Workbench sunset — access ends TODAY, 2026-08-17 (raised 2026-07-17)** — Deadline is the day of this scan. Saved prompts, variables, and evals do not carry to the new Workbench at `platform.claude.com/playground` and must be exported from the banner or Organizational Settings before access ends. Unresolved across four scans; if nothing was exported, whatever was saved there is gone after today. Still Scott's call, and this is the last scan that can precede the cutoff.
- **Experimental prompt tools APIs retired TODAY, 2026-08-17** — `/v1/experimental/generate_prompt`, `/improve_prompt`, `/templatize_prompt` return errors after removal. Still Skip: verified again this scan that no framework skill or Han Solo path calls those endpoints.
- **Opus 5 model-reference sweep (raised 2026-07-24, Act)** — Verified still open by grep this scan, and the target list is **larger** than the 2026-08-10 scan recorded. Confirmed live pins:
  - `han-solo/han_solo/handoff.py:39` → `SYNTH_MODEL = "claude-sonnet-4-6"`
  - `han-solo/han_solo/chat_api.py:204` → `claude-sonnet-4-6` (**not previously recorded**)
  - `han-solo/han_solo/chat_api.py:289` → `claude-sonnet-4-6` (**not previously recorded**)
  - `han-solo/han_solo/runtime/context_assembler.py:84` → `"claude-opus-4-8": 1_000_000` context-window map entry (**not previously recorded** — Opus 5 is also 1M, so this needs an added key, not just a renamed one)
  - `han-solo/han_solo/runtime/harness.py:162` → `claude-opus-4-8` on the test-agent upsert (**not previously recorded**)
  - `han-solo/han_solo/runtime/providers/anthropic.py:13`, `:104` → `claude-opus-4-8` in comments only, not executable config
  - `han-solo/agents/dist/assets/index-Gyez3AXY.js:40` → `claude-fable-5`; grep across `*.py`/`*.ts` found **zero** source occurrences, so this is bundle-only and the source that produced it still has not been located
  - **Correction to the 2026-08-10 scan:** `ren-memory/context.md:140` is prose, not a config pin. The line reads that `claude-opus-4-7` is the coordinator model *in Anthropic's own multiagent examples* — it does not pin Han Solo's coordinator. The prior scan's claim that "any config pinning it with `speed: "fast"` is already broken" has no confirmed instance behind it; no `speed: "fast"` pin on `claude-opus-4-7` was found anywhere in `han-solo`, `ren-memory`, or `Framework Vers1`.
  - Plus the thinking-disable compatibility pass: on Opus 5, `thinking: {"type": "disabled"}` at effort `xhigh` or `max` returns a 400.
- **Managed Agents advisor role and repo-mounted skills (raised 2026-08-07, Watch)** — Both still unresolved, both still Scott's call. No change this week.
- **Dreams model support (raised 2026-07-20, extended 2026-08-01)** — Still unresolved; access form never completed.

---

## Previous Scans

- **2026-08-10** — framework v2.7.0 — 0 Act / 3 Watch / 4 Skip — Managed Agents advisor role (Anthropic formalizing the advisory-reviewer pattern the framework runs by hand) and repo-mounted skill discovery raised as Watch; Opus 4.1 retirement confirmed harmless by grep.
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
