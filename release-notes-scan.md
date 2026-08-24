# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-08-20
**scan_run:** 2026-08-24
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-08-24 scan

Three dated entries published since the last assessed item (2026-08-11): August 18, 19, and 20. Eleven items total — August 19 alone carried seven.

The week's theme is graduation, not new capability: computer use, the Files API, Agent Skills, and the Enterprise Admin API all left beta, and the Python SDK cut a 1.0. Every one of those was verified against the actual code this scan, and none of them force a change. The one genuinely new thing is the browser use tool, and that is a direction question rather than a fix.

### Act

None this week. The Opus 5 model-reference sweep raised 2026-07-24 remains the open Act item — see Carry-Forward, where this scan **located the last unresolved target** and corrects a claim from the 2026-08-17 scan.

### Watch

- **Browser use tool launched (2026-08-19)** — `browser_toolset_20260801`, a client toolset that drives a browser the application hosts: it reads the page's accessibility tree, elements, forms, and tabs, and adds element references, form input, tab management, and download reporting on top of screenshot-and-click. Available for Fable 5, Mythos 5, Opus 5, Sonnet 5, and Opus 4.8. This is the first Anthropic-side capability that could close the framework's documented Live Preview Verification gap — v2.6.0 wired preview verification into design-review Step 1.5 and solo-build Step 1, and `curator-summary.md:122` records that both skip silently outside Claude Code because the Preview MCP tools are Claude Code exclusive. The decision for Scott is whether verification should stay a Claude Code-only capability, or whether Han Solo's runtime should gain browser verification of its own — which would mean routing through a toolset Letta does not expose today, so it is a build, not a config change. There is no consumer now; nothing breaks either way.

### Skip

- **Workbench is now Playground (2026-08-18)** — The Console's Workbench has been replaced by Playground at `platform.claude.com/playground`, supporting every Messages API parameter, with templates and full SDK request/response display. No framework consumer: grep across Framework Vers1 found **zero** references to Workbench in any skill, doc, or template. This entry closes the Workbench carry-forward — see below.
- **Computer use tool out of beta (2026-08-19)** — `computer_toolset_20260801`: no beta header, batch actions, `zoom` on by default, per-member `configs`. Verified no consumer — grep found no `computer_20251124` or `anthropic-beta` reference anywhere in han-solo. Scott's computer use runs through the Claude Code computer-use MCP, a separate surface that this API change does not touch.
- **Files API out of beta (2026-08-19)** — `/v1/files` no longer needs the `files-api-2025-04-14` header; requests without it get `expires_in_seconds` / `expires_at` and `page`/`next_page` pagination. Verified **zero** `/v1/files` references in han-solo; the framework moves files through the filesystem and Postgres, never the Files API.
- **Agent Skills and the Skills API out of beta (2026-08-19)** — `/v1/skills` and `container`-loaded skills no longer need the `skills-2025-10-02` header. Verified **zero** `/v1/skills` references in han-solo: framework skills live in Framework Vers1 and the Han Solo Postgres `agent_skills` table, seeded by `scripts/seed_skills.py`, `seed_framework_skills.py`, and `seed_agent_skills.py`. The question of whether skills should ever be distributed by an Anthropic-native mechanism is already open as the repo-mounted-skills Watch from 2026-08-07 — this GA folds into that decision rather than raising a new one.
- **Admin API user management out of beta (2026-08-19)** — Members, invites, groups, and custom roles for **Claude Enterprise** (claude.ai) organizations; the `ce-user-management-2026-07-13` header is no longer required. Scott is not on Claude Enterprise; nothing here is callable from any framework or Han Solo path.
- **Managed Agents web search and web fetch domain restrictions (2026-08-19)** — `allowed_domains` / `blocked_domains` on the tool's `agent_toolset_20260401` `configs` entry, plus `max_content_tokens` and `user_location`. Han Solo runs on Letta, not the Managed Agents runtime — the same reason this family has been Skip since 2026-07-13.
- **Memory stores attachable in self-hosted sandboxes (2026-08-19)** — SDK workers download each attached store to its `mount_path` and sync changes back. Same reason: Han Solo's four memory stores are Letta and Postgres, with no Managed Agents sandbox in the path.
- **Console session viewer redesign (2026-08-19)** — Timeline minimap, transcript grouped by model request, Inspector panel for cost, raw events, per-tool statistics, and mounted resources. Console UI only; no API or framework behavior changes.
- **Python SDK v1.0 (2026-08-20)** — The HTTP layer moves from `httpx` to `httpx2`; v1.0 requires Python 3.10+, and removes the legacy Text Completions API, the `temperature`, `top_p`, and `top_k` parameters on Messages methods, and the tool runner's `compaction_control`. Async `.with_raw_response` now needs `await response.parse()`, and `AnthropicBedrock` errors instead of defaulting to `us-east-1`. **Verified safe against the only consumer.** `han-solo/scripts/parse_summaries.py:157` is the sole file in han-solo that imports the SDK; it builds a synchronous `anthropic.Anthropic` client and calls `client.messages.create(model="claude-haiku-4-5-20251001", max_tokens=4096, ...)` with no sampling parameters, no raw-response access, and no Bedrock. It runs every 30 minutes under `com.scotth.parse-summaries.plist` on `han-solo/.venv/bin/python3` — Python 3.13.13, anthropic 0.106.0, httpx 0.28.1. Python 3.13 clears the 3.10 floor, and none of the removed surface is touched, so a future `pip install -U anthropic` would not break the summary parser. Han Solo's own server code imports `httpx` directly in nine modules, but that is independent of the SDK's vendored HTTP layer and keeps its own pin. Recorded here so the next upgrade is a decision, not a surprise.

---

## Carry-Forward — dated items still open

- **Opus 5 model-reference sweep (raised 2026-07-24, Act — STILL OPEN)** — Re-verified by grep this scan; every pin below is live, and `claude-opus-5` appears **nowhere** in han-solo source.
  - `han-solo/han_solo/handoff.py:39` → `SYNTH_MODEL = "claude-sonnet-4-6"`
  - `han-solo/han_solo/chat_api.py:204`, `:289` → `claude-sonnet-4-6`
  - `han-solo/han_solo/runtime/context_assembler.py:84` → `"claude-opus-4-8": 1_000_000` in `MODEL_CONTEXT_WINDOW`. Opus 5 is also 1M, so this needs an **added** key, not a renamed one — and note the ceiling is 80% of the window per the LR-9V ruling in that file, so an unknown model id silently falls to whatever the default branch does.
  - `han-solo/han_solo/runtime/harness.py:162` → `claude-opus-4-8` on the test-agent upsert
  - `han-solo/han_solo/runtime/providers/anthropic.py:13`, `:104` → `claude-opus-4-8` in comments only, not executable config
  - **`han-solo/agents/src/components/CreateAgentWizard.tsx:76` — FOUND. Correction to the 2026-08-17 scan**, which recorded `claude-fable-5` as bundle-only with "the source that produced it still has not been located." The source exists; the prior grep used `*.ts` and missed the `.tsx` extension. The line is the agent-creation model dropdown: `anthropic: ['claude-opus-4-8', 'claude-sonnet-5', 'claude-fable-5', 'claude-haiku-4-5-20251001']`. **Opus 5 is not an option in it** — which means every agent created through the wizard today is pinned to a prior generation at the UI layer, and this is the one target on the list that is user-facing rather than internal. It compiles into `agents/dist/assets/index-Gyez3AXY.js:40`, so the bundle needs a rebuild after the source edit.
  - Plus the thinking-disable compatibility pass: on Opus 5, `thinking: {"type": "disabled"}` at effort `xhigh` or `max` returns a 400.
- **Managed Agents advisor role and repo-mounted skills (raised 2026-08-07, Watch)** — Both still unresolved, both still Scott's call. Repo-mounted skills now sits alongside a GA Skills API (2026-08-19), so the two Anthropic-native skill distribution paths are worth deciding on together rather than separately.
- **Dreams model support (raised 2026-07-20, extended 2026-08-01)** — Still unresolved; access form never completed.

### Closed this scan

- **Legacy Workbench sunset (raised 2026-07-17)** — Closed by the 2026-08-18 rename. Access ended 2026-08-17 and Playground is the replacement. Saved prompts, variables, and evals did not carry over; whether anything was exported before the cutoff is not something this scan can determine from the outside. The item is closed because the deadline has passed, not because it was resolved.
- **Experimental prompt tools APIs retired 2026-08-17** — Closed. Re-verified this scan: `generate_prompt`, `improve_prompt`, and `templatize_prompt` appear **nowhere** in han-solo. Nothing broke.

---

## Previous Scans

- **2026-08-17** — framework v2.7.0 — 0 Act / 1 Watch / 2 Skip — Sonnet 5 introductory pricing made permanent at $2/$10, making the Sonnet 5 vs Opus 5 spread a durable 2.5× that the pending model sweep has to decide against; Workbench sunset deadline fell on the scan date.
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
