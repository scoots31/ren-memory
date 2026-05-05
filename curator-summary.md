# Curator Context — Summary Layer

**Purpose:** Fast session orientation. Read this instead of the full curator-context.md (34k tokens).
Read the full file only when a specific framework decision requires the detailed reasoning.

**Full file:** `~/Developer/engineering-playbook/docs/curator-context.md`
**Last synced:** 2026-05-05

---

## Current Framework Version

**v2.4.0** — Design Identity
Released: 2026-05-05

---

## 10 Load-Bearing Principles (what they are, not why — full reasoning in curator-context §2)

1. **Process-first** — to-be map is the contract. Every screen, slice, and test traces to it.
2. **Four anchors required** — design + data + done + process. All four before any code.
3. **Phase gates** — explicit named confirmation, two-option direct question, specific file list.
4. **Always-on ≠ user-invoked** — 4 skills activate at mode selection, run throughout.
5. **Activation taxonomy is fixed** — 7 types: Framework entry, Framework-routed, Phase, On-demand, Auto, Supporting, Always-On.
6. **Continuous capture** — product-continuity captures mid-conversation, not session-end.
7. **Stop on missing prerequisites** — name the gap, name the recovery path. No partial execution.
8. **One sentence of orientation, then start** — framework is invisible.
9. **Output Contract is authoritative in CLAUDE.md** — all skills conform to it.
10. **Framework executes, solo responds** — no commands handed to solo, no files to fill in.

---

## Quality Contract (added v1.4.0–v1.4.1, expanded v1.4.1)

Every slice requires a quality contract written at design review, before code starts. Four categories — each must be addressed or marked N/A with reason:

- Failure states
- Edge cases
- Input validation
- Security

Quality contract is a peer field (`Quality contract:`) alongside `Done criteria:` in the backlog record. Code review Check 8 uses it adversarially. Check 9 is a fixed 5-item security baseline that runs every slice regardless of contract content.

---

## Slice Anchors — Current Format

```
Design anchor: [sprint file] → [element] → [location]
Data anchor: [mock file] → [field names]
Done criteria: [2-3 verifiable statements]
Quality contract:
  - Failure states: [...]
  - Edge cases: [...]
  - Input validation: [...]
  - Security: [...]
Process anchor: [to-be map file] → [step name] → [main path / branch / exception / infrastructure]
```

---

## Metrics (v1.8.0)

`docs/metrics.json` auto-produced by solo-build at first slice. Tracks:
- Rework cycles per slice (increment-only)
- Slices with rework summary
- Phase test refinement cycles
- Reserved: `code_review_flags`, slice-level `refinement_cycles` (not yet wired)

---

## Always-On Skills

| Skill | When | Purpose |
|---|---|---|
| process-mapper | design sprint + design review | Cross-references screens against to-be map |
| product-continuity | all phases | Continuous decision capture |
| framework-health | between phases | Phase gate integrity |
| retrospective | all phases | Flag mode — real-time retro observations |

---

## Session Modes

- **bare** (default) — no routing, no always-on. Skills invoke-only.
- **guided** — full phase chain. Always-on fires. Entry via `start` skill.
- **piloted** — always-on loads once, solo invokes phases manually.

---

## Key Recent Decisions (last 60 days)

| Date | Decision |
|---|---|
| 2026-05-05 | v2.4.0: Design Identity — design-library (1,322 Refero styles + extractor), search.py, design-sprint Step 2 rewritten as library search → 3 options → design-identity.md, design-review reads north star drift |
| 2026-05-05 | Weekly Anthropic release notes monitor — scheduled task Monday 8am, categorizes Act/Watch/Skip, writes to ren-memory/release-notes-scan.md |
| 2026-05-05 | Public APIs reference added to tech-context Q5 — silent check before recording any external API dependency |
| 2026-05-03 | v2.3.0: Security classification — 6 classes declared at tech-context, enforced through design sprint, solo-build pre-flight, and Check 9 (9a–9t class-specific sub-checks). Regulated class gets acknowledgment gate + deploy hard stop |
| 2026-05-03 | v2.1.0: Sub-agent wiring — design-review Enhanced Mode auto (no manual activation), phase-test Stages 4+5 concurrent, design-sprint secondary screens parallel fanout + consistency pass, solo-build code-review as isolated sub-agent |
| 2026-05-03 | v2.0.0: Ren memory infrastructure — ren-memory repo, context.md, curator-summary.md, Stop hooks (diary, push, Cloudflare deploy), daily 8am health check |
| 2026-05-03 | v2.0.0: Comms cascade sub-agent — full skill, auto-commit+push+deploy in one pass |
| 2026-05-02 | v1.9.0: Solo Companion Search + Capture (two-pass MemPalace engine, markdown rendering, write from companion) |
| 2026-05-02 | v1.8.0: metrics.json auto-produced every build. Rework cycle detection. |
| 2026-05-02 | Slice quality scan at plan approval — soft slices rejected before build starts |
| 2026-05-02 | v1.7.0: Automated test generation, CI/CD pipeline, 7 deployment paths |
| 2026-05-02 | client-context-design skill: auto-loads at design sprint when project CLAUDE.md declares client_context path |
| 2026-05-01 | Quality contract — 4-category scaffold required at design review, Check 9 security baseline independent |
| 2026-05-01 | Verbatim quote required at solo-build Step 0 — paraphrase invalid |
| 2026-05-01 | Autopilot: autonomous build mode added |
| 2026-04-30 | Session signal system — 4 skills append signals, Stop hook pushes to shared/session-log.md |
| 2026-04-27 | Framework-wide language pass: directives not suggestions |
| 2026-04-27 | Canonical slice record format established in records-spec |

---

## Dual-Tool Notes (Claude Code vs Cursor)

- Output Contract lives in `~/.claude/CLAUDE.md` (Claude Code) AND `templates/cursor-user-rules-global-playbook.md` (Cursor) — changes must update both
- Agent tool (sub-agents) is Claude Code only — design-review Enhanced Mode is Claude Code exclusive
- Hook system is Claude Code only — Cursor users don't get Stop hook behaviors

---

## Files to Know

| File | Purpose |
|---|---|
| `~/.claude/CLAUDE.md` | Output Contract — authoritative |
| `docs/curator-context.md` | Full reasoning for every framework decision |
| `CHANGELOG.md` | Version history |
| `skills/*/SKILL.md` | Individual skill definitions |
| `docs/communications/` | Blog, skills-reference, guides — deploy to Cloudflare after every change |
| `~/Developer/ren-memory/` | Ren's cross-device session memory |

---

## What to Read the Full curator-context For

- Detailed reasoning behind a specific principle before changing it
- Understanding what a proposed change would break downstream
- Cascade map — which files a given change type affects (§11)
- Full decisions log with rationale (§12)
