# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-08-27
**scan_run:** 2026-08-31
**framework_version_at_scan:** v2.7.0 (from context.md:4 — curator-summary.md still reads v2.6.0 and has been stale since 2026-05-05)

---

## New Items — 2026-08-31 scan

Two dated entries published since the last assessed item (2026-08-20): August 26 and August 27, five items total. Plus one item **the 2026-08-24 scan missed** — the second August 20 bullet, Vertex AI availability for the computer use and browser use toolsets. That prior scan counted eleven items for the Aug 18–20 window but assessed ten; the Google Cloud bullet was the one dropped. It is assessed here and categorized Skip.

The window is administrative — Console key types, Compliance API graduation, Admin API SDK coverage, and SDK beta-namespace cleanup. Every item was grepped against han-solo and Framework Vers1 this scan. **No consumer exists for any of them**, and nothing forces a framework change.

### Act

None this week.

The Opus 5 model-reference sweep raised **2026-07-24 remains the open Act item, now five weeks old** — re-verified by grep this scan and still entirely unresolved. See Carry-Forward.

### Watch

None this week.

The browser use tool (raised 2026-08-24) is still Scott's call and is unchanged in substance; the Google Cloud item below widens where it is available but does not alter the decision.

### Skip

- **SDK beta namespaces aligned for Files and Skills (2026-08-27)** — In Python SDK 1.2.0 (plus TypeScript 0.122.0, Go 1.68.0, Java 2.59.0, Ruby 1.67.0, C# 12.44.0), `client.beta.files` and `client.beta.skills` stop sending the `files-api-2025-04-14` and `skills-2025-10-02` headers and return the same shapes as `client.files` / `client.skills`. Two behavior changes ride along: `client.beta.skills.delete()` now deletes a Skill together with all its versions, and the beta Messages type `BetaSkill` is renamed `BetaContainerSkill`. Requests still sending the beta headers keep the old shapes. **Verified zero consumers** — grep across han-solo for `beta.files`, `beta.skills`, `BetaSkill`, `beta.organization`, `client.files`, and `client.skills` returned nothing. The sole SDK consumer remains `han-solo/scripts/parse_summaries.py:157`, which builds a synchronous `anthropic.Anthropic` client and calls `client.messages.create(model=MODEL, max_tokens=4096, messages=[...])` — no Files, no Skills, no beta namespace. Re-confirmed installed: anthropic 0.106.0 on Python 3.13.13 in `han-solo/.venv`. The version-delete change is the one with teeth if a Skills consumer ever appears; recorded so that is a decision rather than a surprise.
- **Personal keys and service account keys in the Console (2026-08-27)** — Keys can now be created that act as you or as a [service account](workload identity federation), carrying the same permissions and ceasing to work when the linked account leaves an organization. They can be workspace-scoped or admin/cross-workspace. **Workspace API keys are now labeled a legacy option.** No framework text becomes wrong: grep found three files touching Claude API auth, and none prescribes a workspace key. `Framework Vers1/skills/tech-context/SKILL.md:241` records the local pattern as `ANTHROPIC_API_KEY` in an env var, and `docs/communications/guide-build.html:1929` plus `blog.html:257` already route **cloud** deployments to Workload Identity Federation rather than a static key. That guidance is unaffected and, if anything, reinforced. This is a Console key-creation change, not an API behavior change, and Scott is not administering an organization.
- **Compliance API session endpoints out of beta (2026-08-26)** — The session endpoints graduate for Cowork and Claude Code sessions. Claude Enterprise only, and grep found **zero** `v1/compliance` references anywhere in han-solo. Same reason this family has been Skip since 2026-08-03: Scott is not on Claude Enterprise, so nothing here is callable from any framework or Han Solo path.
- **Compliance API adds Claude Science and Microsoft 365 sessions (2026-08-26)** — Local session endpoints now also return Claude Science transcripts (`product_surface` = `claude_science`) and Claude for Microsoft 365 sessions in Excel, PowerPoint, Word, and Outlook (`product_surface` beginning `office_agents`), in beta for Claude Enterprise with the existing Compliance Access Key and `read:compliance_user_data` scope. Same Enterprise gate, same absence of a consumer.
- **Admin API reaches the CLI and SDKs (2026-08-26)** — `client.beta.organization` in the `ant` CLI and the Python, TypeScript, C#, Go, Java, PHP, and Ruby SDKs, covering org info, members, invites, workspaces, API keys, rate limits, service accounts, workload identity federation issuers and rules, and CMEK. Usage/cost reports and the Claude Enterprise user-management and analytics endpoints stay curl-only. Verified **zero** Admin API usage in han-solo. Worth one line of forward-looking note only because the coverage list includes workload identity federation issuers and rules — the mechanism the framework's own cloud-deploy guidance points at — so if WIF setup ever moves from manual Console steps into the deploy step, this is the programmatic path. No action today; no consumer.
- **Computer use and browser use toolsets on Google Cloud (2026-08-20 — missed by the prior scan)** — `computer_toolset_20260801` and `browser_toolset_20260801` are now available on Vertex AI for Fable 5, Mythos 5, Opus 5, Sonnet 5, and Opus 4.8, using the same `tools` entries as the Claude API. Grep found no `AnthropicVertex`, `vertex`, or `aiplatform` reference in han-solo — Han Solo runs on Letta against the Claude API directly. Skip on availability grounds, but it is the second cloud to carry the browser toolset, which slightly strengthens the case in the open browser-use Watch that this is becoming a durable platform capability rather than a Claude API-only experiment.

---

## Carry-Forward — dated items still open

- **Opus 5 model-reference sweep (raised 2026-07-24, Act — STILL OPEN, five weeks)** — Re-verified by grep this scan. `claude-opus-5` appears **nowhere** in han-solo source (excluding stale worktree copies and build output). Every pin below is live and unchanged since the 2026-08-24 scan:
  - `han-solo/han_solo/handoff.py:39` → `SYNTH_MODEL = "claude-sonnet-4-6"`
  - `han-solo/han_solo/chat_api.py:204`, `:289` → `claude-sonnet-4-6`
  - `han-solo/han_solo/runtime/context_assembler.py:84` → `"claude-opus-4-8": 1_000_000` in `MODEL_CONTEXT_WINDOW`. Opus 5 is also 1M, so this needs an **added** key, not a renamed one — and the ceiling is 80% of the window per the LR-9V ruling in that file, so an unknown model id silently falls to whatever the default branch does.
  - `han-solo/han_solo/runtime/harness.py:162` → `claude-opus-4-8` on the test-agent upsert
  - `han-solo/han_solo/runtime/providers/anthropic.py:13`, `:104` → `claude-opus-4-8` in comments only, not executable config
  - `han-solo/agents/src/components/CreateAgentWizard.tsx:76` → `anthropic: ['claude-opus-4-8', 'claude-sonnet-5', 'claude-fable-5', 'claude-haiku-4-5-20251001']`. **Opus 5 is still not an option in the wizard dropdown**, so every agent created through the UI today is pinned to a prior generation. This is the one user-facing target on the list. It compiles into `agents/dist/assets/index-Gyez3AXY.js`, so the bundle needs a rebuild after the source edit.
  - Plus the thinking-disable compatibility pass: on Opus 5, `thinking: {"type": "disabled"}` at effort `xhigh` or `max` returns a 400.
- **Browser use tool (raised 2026-08-24, Watch)** — `browser_toolset_20260801`, now on both the Claude API and Google Cloud. The decision for Scott is unchanged: whether live preview verification stays a Claude Code-only capability, or whether Han Solo's runtime gains browser verification of its own. v2.6.0 wired preview verification into design-review Step 1.5 and solo-build Step 1, and `curator-summary.md:122` records that both skip silently outside Claude Code. Routing through this toolset would mean going around what Letta exposes today — a build, not a config change. There is no consumer now; nothing breaks either way.
- **Managed Agents advisor role and repo-mounted skills (raised 2026-08-07, Watch)** — Both still unresolved, both still Scott's call. Repo-mounted skills sits alongside a GA Skills API (2026-08-19) and now a GA SDK surface for it (2026-08-27), so the Anthropic-native skill distribution paths are worth deciding on together rather than separately.
- **Dreams model support (raised 2026-07-20, extended 2026-08-01)** — Still unresolved; access form never completed.

### Closed this scan

None. No carry-forward item was resolved or expired during this window.

---

## Previous Scans

- **2026-08-24** — framework v2.7.0 — 0 Act / 1 Watch / 9 Skip — a graduation week (computer use, Files API, Agent Skills, Enterprise Admin API all left beta; Python SDK cut 1.0, verified safe against `parse_summaries.py`); browser use tool raised as Watch against the Live Preview Verification gap; located the last unresolved Opus 5 target in `CreateAgentWizard.tsx` and corrected the prior scan's claim that it could not be found. **Note: this scan assessed ten of eleven items — the Aug 20 Vertex AI bullet was missed and is assessed in the 2026-08-31 scan.**
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
