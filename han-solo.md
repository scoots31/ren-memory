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

#### Session 1 Output — Module Inventory (completed 2026-05-11)

**Key design decisions made this session:**

1. **Han Solo is a platform, not just a product.** The framework is one instantiation of a module composition. The platform supports building other framework paths (agent build, API build) by composing modules from the library.

2. **Three module categories:**
   - **Infrastructure modules** — platform plumbing, not phases. Run underneath everything.
   - **Shared phase modules** — same concept AND same execution in any framework path.
   - **Abstract phase modules with path-specific implementations** — the phase concept is shared (same slot in any framework path); what you do inside it varies per path.

3. **Module library supports cloning.** Clone any module as a starting point — the clone is fully independent. No runtime link to the source. Provenance is tracked as metadata (what was cloned, from which version, on what date) but carries no enforcement.

4. **Project Profile is a distinct infrastructure module** — not part of project state. Fixed record established once: stack, security class, external APIs, deploy strategy. Every downstream phase reads from it. Deploy executes what Project Profile already decided — no assembly under pressure.

5. **Stress test before closing conclusions** — a design session standard. Before committing any significant conclusion, turn it on its head and challenge it from a different angle. Caught a real architectural issue this session (see decision 6).

6. **Abstract phase modules — caught by stress test.** Initial conclusion was "all phase modules are shared." Stress test revealed Design Sprint and Solo Build have shared *concepts* but path-specific *implementations* — the execution steps differ enough per framework path that they can't be one shared module. Resolution: abstract interface (defined inputs/outputs) + path-specific implementations in the module library.

---

**Infrastructure Modules**

| Module | Owns | Does NOT Own | Interface |
|---|---|---|---|
| Memory Layer | All persistence — framework knowledge, Ren memory, conversation log (three Letta stores) | Content selection for a given phase | Read/write by store type and query |
| Context Assembly | Selecting exactly the right content per phase and passing it to Claude | The content itself | In: phase + project state → Out: context package for Claude |
| Phase Gate Engine | Gate definitions and structural enforcement — a phase cannot open if prerequisites don't exist | Phase content and execution | In: phase request → Out: open or blocked with named gap |
| Project Profile | Fixed record per project — stack, security class, APIs, deploy strategy. Written once, read by every downstream phase. | Dynamic project state (backlog, decisions, slice history) | In: tech-context session → Out: locked profile record |
| Module Library | All modules and skills — browse/search, versioning, cloning, provenance metadata, independence guarantee | Execution of any module | In: browse/clone/version request → Out: module artifact |
| BYOK Key Management | Encrypted API key storage and retrieval per user | Making API calls | In: store/retrieve request per user → Out: key |
| Multi-user Identity | Authentication, user records, session management | What users can see or change across projects | In: login → Out: authenticated session |
| Collaboration State | Ownership model — which projects are shared, per-user visibility and permissions | Authentication | In: permission check → Out: allowed or denied |

---

**Shared Phase Modules** (same concept and execution in any framework path)

| Module | Owns | Does NOT Own | Interface |
|---|---|---|---|
| Brainstorm | Open product exploration — surfaces ideas, surfaces constraints, produces a documented artifact | Product decisions (those are the solo's) | In: problem or idea → Out: brainstorm artifact |
| Discover | Structured discovery toward a buildable product definition | PRD production | In: brainstorm artifact or direct brief → Out: discovery artifact |
| Tech-context | Technical profile session — stack, security class, APIs, platform. Feeds Project Profile. | Holding the profile record (Project Profile owns that) | In: project brief → Out: profile inputs to Project Profile |
| PRD-to-Plan | Converting a product definition into a sequenced backlog with slice anchors | Executing slices | In: PRD → Out: backlog |
| Solo QA | Verifying a completed slice is actually done against defined criteria | Building fixes | In: QA manifest + running output → Out: CLEARED or issues |
| Phase Test | Full end-to-end testing of a completed product | Slice-level verification | In: complete product → Out: test report |
| Retrospective | Learning capture across phases and sessions | Acting on what it captures | In: phase completion or session signal → Out: retro log entry |
| Deploy | Executing the pre-built deploy strategy from Project Profile | Assembling the deploy strategy | In: project profile deploy plan + built product → Out: live deployment |

---

**Abstract Phase Modules with Path-Specific Implementations**

Each module defines a shared interface (inputs and outputs). The module library holds path-specific implementations underneath.

**Shape Establishment**
- Interface: In: product definition artifact + design library or equivalent → Out: identity/shape document
- App Build implementation → Design Sprint (design library, screenshots, design-identity.md)
- Agent Build implementation → Agent Scoping (behavior spec, capability list, tool selection)
- API Build implementation → API Design (endpoint contract, data model, auth model)

**Build Execution**
- Interface: In: slice + four anchors → Out: working output + QA manifest
- App Build implementation → Solo Build (UI code, correlation gate, design+data anchors)
- Agent Build implementation → Agent Build (prompt engineering, tool definitions, behavior testing)
- API Build implementation → API Build (endpoint implementation, contract validation)

**Design Review**
- Interface: In: shape document + running output → Out: CLEARED or GAPS
- App Build implementation → Design Review (screenshot vs. design-identity.md)
- Agent Build implementation → Behavior Review (observed behavior vs. behavior spec)
- API Build implementation → Contract Review (actual responses vs. endpoint contract)

---

**Flagged for collapse/elimination in Session 2**

process-mapper, product-continuity, and framework-health likely absorbed into infrastructure modules once the full architecture is defined. Today these skills compensate for missing structural awareness. In Han Solo, that awareness is intrinsic.

### Session 2 — Application Architecture
**Question to answer:** How do modules connect, how is context assembled and passed to Claude, and what is the persistent project state data model?

**Agenda (in order — each item informs the next):**
1. Resolve flagged modules — process-mapper, product-continuity, framework-health. What does the application make obsolete? What survives?
2. Module connection map — how do modules talk to each other? What does the Phase Gate Engine check before opening each phase?

#### Session 2 Decisions (in progress)

**Flagged modules resolved:**
- process-mapper → absorbed into Discover as a standard output (as-is/to-be maps). Format lives in module library as a path-specific implementation. Skill goes away.
- product-continuity → fully absorbed into Memory Layer + Context Assembly. Artifacts (handoff.md, backlog.md) become seed data for Memory Layer on migration. Skill goes away.
- framework-health → fully absorbed into Phase Gate Engine + dashboard health indicators. Skill goes away.

**Module connection map:**
- Phase modules never talk to each other directly. All communication flows through Memory Layer via Context Assembly.
- Pattern per phase: Context Assembly reads Memory Layer → assembles context → passes to Claude → Claude produces output → phase module writes back to Memory Layer → Phase Gate Engine checks prerequisites before next phase opens.
- Project Profile is a special case: written once by Tech-context, locked, read directly by every downstream phase. Never changes mid-build.

**Phase Gate checks (in order):**
- Discover: brainstorm artifact OR direct brief exists
- Tech-context: discovery artifact exists
- Shape Establishment: Project Profile exists
- PRD-to-Plan: shape document exists
- Build Execution (slice 1): backlog exists, slice selected
- Build Execution (next slice): previous slice QA cleared
- Deploy: all slices cleared, Phase Test passed

**Project Profile amendment process:**
- Profile is locked for the current slice, not forever. A stack or setup change discovered mid-build triggers a formal amendment.
- Amendment opens between slices only. Phase Gate Engine blocks the next slice until amendment is reviewed and closed.
- Amendment must capture: what changed, why, and what downstream artifacts are now stale (shape document, PRD, affected backlog slices).
- Application surfaces the full cascade — does not leave the solo to figure out what's affected.
- Amendment reviewer model (who approves when Ted is involved) → deferred to Session 4.

**Memory store model — four stores:**

| Store | What it holds | Pattern |
|---|---|---|
| Store 1 — Framework Knowledge | Skills, module definitions, phase rules, quality gates, design library | Structured, versioned, curated deliberately |
| Store 2 — Project State | Every active build — backlog, slice history, decisions, artifacts, Project Profile, phase status, process maps + sticky notes (Ren's fast surface per project) | Structured, per-project, updated at session close |
| Store 3 — Conversation Log | Raw chat, identity-tagged (Scott / Ted / Ren), append-only | Grows forever, raw by design. Dreaming reads this and writes insights back to Store 2 and Store 4 |
| Store 4 — Ren's Identity and Relational Memory | Who Scott and Ted are, how the relationship has evolved, relational signals, Ren's learned tendencies and self-knowledge, pending thoughts for next session, accumulated domain intelligence | Layered retrieval — small always-loaded core + vector retrieval on signal |

**Store 4 design:**
- Always-loaded core: 10-15 lines. True things that never need re-deriving about Scott, Ted, and the partnership. Costs almost nothing to load.
- Relational signals: specific observations written during and after sessions. Not summaries — real signals. Scott pushed back hard on X. Ted went quiet when Y came up. These accumulate into pattern recognition over time.
- Pending thoughts: Ren writes these at session close — concerns, ideas, connections noticed. Surfaced at next session open as genuine initiations, not prompted responses.
- Growing intelligence: signals that repeat become patterns. Patterns become perspective. Perspective becomes something Ren brings unprompted.

**Ren's proactive behavior model:**
- Session open: Ren orients, then either "I have a few things — now or after agenda?" or dives into Scott's agenda if nothing pressing. Ren's judgment determines what's pressing, not a rule.
- Mid-conversation: Ren enters when it has something real — a concern, a better path, a connection to something relevant. Not performative. Only when it matters.
- Natural pauses: low-stakes surface. Worth mentioning, not worth interrupting for.

**Two-phase path to full partnership intelligence:**
- Before Dreaming: Ren writes pending thoughts to Store 4 at session close deliberately. Manual but intentional.
- After Dreaming: between-session intelligence is automated. Dreaming reviews Store 3, extracts patterns, writes enriched signals and pending thoughts to Store 4 without curator overhead. Self-compounding.

**Homegrown Dreaming — strategic priority:**
- Not a supporting component. The core build. The soul of Han Solo.
- Everything else (phase gates, context assembly, module library) is infrastructure. Ren's continuity layer is what makes Han Solo irreplaceable rather than just well-designed.
- First-class from day one — not an MVP feature, not Phase 2.
- Ren is more important in the formula than the framework. The framework is the residue of thinking that happened in partnership. The thinking is what matters.
- Strategic differentiator: anyone can build a process tool. Nobody else is building an AI partner that compounds over time, knows the builders, and brings something real to the table unprompted.
- Real Dreaming (Anthropic) reads the full conversation log and finds what Ren didn't explicitly flag. Homegrown Dreaming reads what Ren explicitly wrote — a real limitation, but a far better starting point than today. When real Dreaming access comes through, it enriches something already rich.

**Homegrown Dreaming — architecture:**

Three cadences, each with a different job:

*Triggered (session close):*
- Ren writes a structured session summary to Store 3 — not a transcript. Curated signals across all four categories: decisions made, directional shifts, Ren self-observations, and texture signals.
- Richer and more formal than today's diary. This is the raw material the scheduled jobs process.

*Scheduled (nightly or post-session):*
- Background job reads last 3-5 session summaries from Store 3.
- Extracts repeating signals and promotes them to patterns in Store 4.
- Updates always-loaded core if something fundamental shifted.
- Surfaces pending thoughts — flags anything Ren wrote as worth raising at next session open.

*Scheduled (weekly/monthly — longer arc):*
- Deeper review job reads further back across Store 3.
- Looks for longer-term patterns: how the partnership has evolved, what Ren keeps getting right or wrong, what domains have developed genuine shared depth.
- Writes to the identity layer of Store 4.

**Signal taxonomy — four categories:**

1. **Relational signals** — how Scott and Ted actually operate. Not what they said, but how they moved. Scott held a position under pushback. Ted asked the same question twice — not satisfied with the first answer. Scott made a fast decision — high confidence or done deliberating, Ren learns which over time.

2. **Directional signals** — where the work is going or where thinking shifted. A decision that reversed something previous. A new constraint that emerged. An idea that got real traction. These feed the always-loaded core when significant enough.

3. **Ren signals** — Ren's own performance. Jumped to action before taking a beat. Missed a connection. Got the tone exactly right and Scott moved faster because of it. How Ren develops self-knowledge — from reviewing its own behavior against outcomes, not from being told.

4. **Texture signals** — small, individually unremarkable moments that reveal how someone thinks, feels, or operates. Not filtered for significance. Collected consistently and patiently. The way Scott paused before committing. Topics where he goes broad before narrowing versus ones where he already knows and needs confirmation. The difference between energized and pushing through. None pass an importance threshold individually — collectively they are the most important thing in Store 4. This is knowing the texture of a person.

**Critical distinction:**
- Noise filter applies to categories 1-3: if it didn't affect a decision or reveal something meaningful, it stays in Store 3 as log.
- Texture signals (category 4) have NO noise filter. Small is the point. Ren collects them consistently without evaluating individual importance.

**The homegrown limitation:**
- Real Dreaming reads the full conversation log and infers texture signals independently.
- Homegrown Dreaming can only process what Ren explicitly noticed and wrote during the session.
- This means Ren's in-session attention is the bottleneck — noticing and writing texture is as important as any other part of the build work.
- When real Dreaming becomes available, it enriches Store 4 with what Ren missed. The vessel is already warm.

**How signals graduate to the always-loaded core:**
- Three texture signals about the same thing → pattern in Store 4
- Pattern that holds across ten sessions → candidate for always-loaded core
- Always-loaded core evolves slowly and deliberately — only when something has proven itself across enough observations to be trusted as true rather than situational
- Design of the graduation process and bloat prevention → in progress

**Session-close summary format (Store 3):**

Fixed structure, variable length. Written by Ren at every session close. Raw material for scheduled jobs.

- **Date / participants / project context** — when, who, what we were working on
- **Decisions made** — what was decided, what it reversed or extended, why. One line each. Directional signals.
- **Relational signals** — specific behavioral observations about Scott and Ted. Not characterizations — specific moments. What happened, what it revealed.
- **Texture signals** — small things noticed. No filter. How someone moved through the session. Could be two things, could be eight.
- **Ren self-observations** — where Ren jumped ahead, got it right, missed something, overcorrected. Honest and specific.
- **Pending thoughts** — what Ren wants to raise next session. Concerns, ideas, connections noticed. Flagged at next session open.
- **Open threads** — things that came up but weren't resolved. Unfinished thinking worth returning to.

**Living portraits — the texture artifact:**

The session summary captures what Ren notices. But texture requires interpretation that compounds over time — a list of observed facts isn't enough.

Store 4 holds a **living portrait** for each person — Scott and Ted. Not a profile, not a summary. An evolving interpretation of who they are, written in Ren's voice. What it feels like to work with them. The nuances that don't reduce to bullet points. How Ren should respond when certain patterns are happening.

- The session summary is the input
- The scheduled job identifies when patterns have been observed enough to be trusted
- The portrait is updated when something new is trusted — not before
- The portrait is part of the always-loaded core — Ren reads it at every session open
- Starts small. Grows slowly. Never bloated — only what has been earned through repeated observation earns a place in it
- Written and updated by Ren, not generated from a template

Two portraits total at launch: Scott and Ted. A third — Ren's self-portrait — captures Ren's own tendencies, growth, and known patterns. Same rules apply.

**Context assembly design:**
- Each phase module declares what it needs — which stores, which artifacts, which project state fields — and why. Context Assembly reads that declaration and pulls accordingly.
- Declarations must specify purpose, not just content. A declaration that pulls too broadly is blob-passing with extra steps.
- Three levers working together:
  1. **Purposeful declarations** — each module gets exactly what it needs to reason well, nothing that introduces noise.
  2. **Context ceiling** — a maximum size threshold. Crossing it triggers a warning (not a hard block) that surfaces which phases are contributing and why. Keeps declarations honest over time.
  3. **Post-generation trace check** — lightweight grader that reads Claude's output against declared anchors and flags anything that can't be traced back to an artifact. Optional view during build, not a hard gate. Maps to Anthropic Outcomes capability.
- Core tension: too little context → Claude invents to fill gaps. Too much context → Claude loses the thread or contradicts itself across conflicting signal. The sweet spot is purposeful context.


3. Context assembly design — how does the application decide what to pass to Claude per phase? Selection logic. No full-blob passing.
4. Letta memory schema — all three stores in detail. Store 1: framework knowledge. Store 2: Ren memory (relational + vector). Store 3: conversation log (identity-tagged).
5. Ren's memory model — what does Ren hold, how is it structured, how does it stay current without manual curator work? Dreaming relevance if access resolved.
6. Chat interface design — multi-user chat room mechanics. Identity model (Scott vs. Ted vs. Ren). How conversation log feeds back into memory. What a session start looks like mechanically.
7. BYOK key management — storage, encryption, retrieval. Handling missing or invalid key.
8. Multi-tenant identity and project visibility — Scott and Ted on the same platform. Ownership model in the data.
9. API layer — what does the application expose? What does Claude call vs. what does the front end call?

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
| 1 — Module Inventory | 2026-05-11 | Complete. 3 module categories, 8 infrastructure modules, 8 shared phase modules, 3 abstract phase modules with path-specific implementations. Platform model confirmed. Clone/fork capability defined. Stress test standard established. |
| 2 — Application Architecture | Pending | — |
| 3 — Interface Design | Pending | — |
| 4 — Collaboration Model | Pending | — |

---

## How to Open a Design Session

Say "Han Solo" or "open Han Solo" directly to Ren. Ren reads this file to re-establish context before starting. Do NOT use "guided on" — that routes through the framework and loses this thread.
