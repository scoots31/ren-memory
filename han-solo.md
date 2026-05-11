# Han Solo — Design Reference

**Codename:** Han Solo — private to Scott and Ren only. Never referenced in engineering-playbook or any shared artifact.
**Created:** 2026-05-11
**Status:** Pre-build. Four design sessions required before any build begins.

---

## The Strategic Problem

The framework today is a build-time tool living inside Claude Code's context model — CLAUDE.md files, skills loaded at runtime, session state that evaporates between conversations. The vulnerability: Anthropic or Cursor ships native memory and workflow orchestration that makes the CLAUDE.md + skills pattern feel dated. The framework becomes a power-user configuration of someone else's tool rather than a product in its own right.

The answer is to build an owned application that holds all framework overhead — skills, rules, project state, continuity docs — and passes precisely the right context to Claude via API at the right moment. Claude is the reasoning engine. The application is the orchestrator and the brain.

---

## Core Vision

An owned application that:
- Holds the full picture of every active product continuously — no more reconstructing context from scratch each session
- Assembles exactly the right context for the current task (design-review gets design identity + PRD + open slices, nothing else)
- Makes phase gates structural, not instructional — the application cannot open a build phase if tech-context doesn't exist
- Supports Scott and Ted as co-builders on shared project state
- Owns the full product lifecycle, not just the build phase

**Business model:** BYOK (bring your own API key). Users bring their own Claude key — lowers infrastructure cost, shifts API spend to users. Proven model in AI tooling.

**Distribution:** Private first — Scott and Ted as first users. May open to outside users over time. May stay private permanently. Both are valid outcomes.

---

## What Improves Over the Current Framework

| Problem today | How the application fixes it |
|---|---|
| Session state evaporates | Application holds full project state continuously — never loses context |
| CLAUDE.md loads a large blob regardless of task | Application assembles exactly the right context per phase |
| Phase gates are instructional (Claude follows rules) | Phase gates are structural (application enforces them) |
| Multi-project visibility requires visiting each project's files | All active builds, phases, health visible in one place |
| Skill improvements only apply to future projects | Skill versioning propagates across all active projects |
| No story for live products post-deploy | Application holds the full product lifecycle — not just "build this slice" |
| Scheduled tasks are macOS LaunchAgent hacks | Owned by the application |
| Solo Companion is a Cloudflare Worker add-on | Central interface, first-class |

**The drift problem is addressed:** Claude doesn't hold state in this model — the application holds state and feeds it precisely when needed. The application never compacts, never forgets, never drifts. Claude operates on clean signal assembled from an authoritative source.

---

## Architecture Decisions (Locked)

**Decided:** 2026-05-09

| Decision | Value |
|---|---|
| Hosting | Render (~$21/mo — Letta service + PostgreSQL + web app) |
| Memory foundation | Letta self-hosted (Apache 2.0, open source, no limits, fully modifiable) |
| Memory store 1 | Framework knowledge — document store |
| Memory store 2 | Ren memory — relational + vector |
| Memory store 3 | Conversation log — identity-tagged raw chat |
| All stores | PostgreSQL on Render |
| Primary interface | Multi-user chat room — Scott, Ted, Ren |
| CLAUDE.md evolution | Stays local as a thin bridge for local execution work. Framework context assembly moves to Han Solo cloud. |
| Memory seed on migration | ren-memory + MemPalace — cleanup pass before migrating to avoid carrying stale content |

---

## Managed Agents Relevance (Anthropic — 2026-05-07)

Four capabilities released that map directly to Han Solo's pillars:

| Capability | Status | Han Solo Connection |
|---|---|---|
| **Outcomes** | Public Beta | Quality contract enforcement as a first-class API. Rubric-driven grader iterates until satisfied. |
| **Multiagent Orchestration** | Public Beta | Sub-agent orchestration with real shared filesystem (`/mnt/session/`). Persistent threads. 25-thread limit, 1-level-deep delegation. |
| **Webhooks** | Public Beta | Push notifications for session state changes — direct path to event-driven Solo Companion updates, no polling. |
| **Dreaming** | Research Preview | Background process that reviews past sessions, extracts patterns, curates memory between sessions. Closes the open question: how does the owned application get smarter over time without curator work after every session? |

**Dreaming is the most strategic piece.** Today, what the framework learns between sessions is whatever Scott and Ren manually write into MemPalace and ren-memory. Dreaming automates that layer.

**Dreaming access status:** Form signup required at claude.com/form/claude-managed-agents. Form rejects ProtonMail and Gmail. Scott is pursuing X outreach to Anthropic. Not a blocker for design sessions — it's design input, not a prerequisite.

**SDK note:** `claude-opus-4-7` is the coordinator model in Anthropic's own multiagent examples.

---

## Defensible Moat

Anthropic will improve memory. Cursor will improve project context. Neither is building an opinionated, process-enforcing, lifecycle-managing product layer for solo builders. The only way to be displaced is if someone builds the same product with the same process depth — which means replicating years of framework decisions, quality gates, design discipline, and continuity model. Copying skill files from GitHub gets them the prompts. It doesn't get them the product or the judgment behind it.

---

## What Han Solo Owns vs. What It Delegates

**Han Solo owns:** continuity layer, build history, product intelligence, collaboration state, scheduled tasks, phase enforcement.

**Existing tools own:** production ops (Datadog, Sentry), task management (GitHub Issues, Linear), code (GitHub). Don't rebuild what those do well.

---

## The Four Design Sessions

In order. None can be skipped. Build doesn't start until all four are complete and approved.

### Session 1 — Module Inventory
**Question to answer:** What are the formal modules of this application, what does each one own, and what is each one's interface?

Formally name every existing module (drawn from the framework's current skills, phases, and continuity model). Define what each module owns. Define how modules interact. This is the foundation for everything else — Session 2 cannot start until every module is named and bounded.

### Session 2 — Application Architecture
**Question to answer:** How do modules connect, how is context assembled and passed to Claude, and what is the persistent project state data model?

Covers: how context is assembled per phase, BYOK key management, Letta memory store schema, persistent project state data model, multi-tenant identity model (Scott + Ted), API layer design. **Validate multi-user identity model first** — it's the riskiest assumption.

### Session 3 — Interface Design
**Question to answer:** What does the application look like to use?

Solo Companion is the seed. Multi-user chat room is the primary interface. What does the full surface look like? What does a session start feel like? How does multi-project visibility work? What does phase enforcement look like from the user's perspective?

### Session 4 — Collaboration Model
**Question to answer:** How do Scott and Ted operate together?

Ownership model across shared and separate projects. Visibility — what can each user see about the other's projects? Permissions — what can each user change? How are shared framework skills versioned and controlled vs. project-specific overrides?

---

## Open Questions (Unresolved)

These are the questions that need answers before or during the design sessions. Not blockers for starting Session 1 unless noted.

- What is the seed interface — Solo Companion evolved, or built fresh?
- How does skill versioning work — per-user override capability or framework-controlled canonical?
- What does the multi-project dashboard actually show? Phase, health, last activity, open decisions?
- How does BYOK key management work — stored encrypted per user, or passed per session?
- Does this replace the engineering-playbook flat files or live alongside them during a transition period? (The local CLAUDE.md stays as a thin bridge — but when does that bridge become vestigial?)
- Dreaming access — if resolved before Session 2, it changes the memory architecture conversation.

---

## Session Log

| Session | Date | Outcome |
|---|---|---|
| 1 — Module Inventory | Pending | — |
| 2 — Application Architecture | Pending | — |
| 3 — Interface Design | Pending | — |
| 4 — Collaboration Model | Pending | — |

---

## How to Open a Design Session

Say "Han Solo" or "open Han Solo" directly to Ren. Ren reads this file to re-establish context before starting. Do NOT use "guided on" — that routes through the framework and loses this thread.
