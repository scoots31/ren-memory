# Framework Design Ideas

Ideas logged for future design sessions. Not approved for execution yet.

---

## Marketing Workflow (Post-Deploy Skill)

**Logged:** 2026-05-05
**Status:** Design session needed — not next, not scheduled

### The Problem It Solves

Vibe coders build apps and make no money because they don't know how to market or sell them. The framework already produces all the raw material a marketer would spend weeks extracting from a founder — but no one ever synthesizes it into a message.

### The Idea

An optional post-deploy skill that reads existing framework artifacts and produces marketing outputs across channels:
- Positioning statement (the anchor — one true thing for one specific person)
- Landing page copy
- App Store / web store description
- Social media posts (platform-appropriate)
- One-pager (print / shareable)

Everything derives from the positioning statement, not generated in parallel as bulk content.

### Why the Framework Is Uniquely Positioned for This

The raw material already exists:
- **Discover** — user problem, who it's for, the pain
- **PRD** — what it does, why it matters, target user
- **Design Identity** — north star, visual language, brand feel
- **Backlog** — full feature set, what shipped
- **Handoff** — shipping record, known constraints

This is more grounded than any generic "write me marketing copy" prompt. The framework did the thinking — the skill just synthesizes it.

### Design Principles (agreed 2026-05-05)

1. **Positioning statement first.** One tight anchor. Everything else derives from it.
2. **Optional and explicit.** Not a default phase. Solo opts in post-deploy.
3. **Channel outputs flow from anchor.** Print, web, social are derivations — not parallel bulk generation.
4. **Quality gated by discover depth.** Weak discover → weak positioning. The skill should flag if discover artifacts are thin before generating.

### Open Questions for Design Session

- Does this live as a standalone skill, or as an extension of the deploy skill?
- What's the right output format for social posts? Platform-specific variants or raw copy to adapt?
- Print: one-pager only, or press kit (bio, screenshots, fact sheet)?
- Should it produce a `docs/marketing/` directory with versioned outputs alongside the product docs?

### Related Context

Scott observed: "I read a lot of articles that vibe coders build these apps and have not made no money off of any of them because they don't know how to market or sell it." The framework already addresses the underlying product-market fit problem better than a marketing skill would — but there's a real gap between shipping something good and having anything to say about it publicly.

Shipper.now (discussed same session) is a competitor in the broader space — their differentiation attempt is "The Advisor" for strategy/market positioning. The framework's version would be grounded in actual product artifacts, not generic AI guidance.

---

## Framework as Owned Application (Strategic Product Vision)

**Logged:** 2026-05-05
**Status:** Design session needed — not next, not scheduled
**Trigger:** Ted's principle — "think about the type of company that will put you out of business and become that company"

### The Strategic Problem

The framework today is a build-time tool living inside Claude Code's context model — CLAUDE.md, skills loaded at runtime, session state that evaporates. The vulnerability: Anthropic or Cursor ships native memory and workflow orchestration that makes the CLAUDE.md + skills pattern feel dated to new users. The framework becomes a power-user configuration of someone else's tool rather than a product in its own right.

### The Idea

Build an owned application that holds all framework overhead — skills, rules, project state, continuity docs, documentation — and passes precisely the right context to Claude via API at the right moment. Claude is the reasoning engine. The application is the orchestrator and the brain.

**Business model:** BYOK (bring your own API key). Users bring their own Claude key — lowers infrastructure cost, shifts API spend to users, aligns incentives. Proven model in AI tooling.

**Distribution:** Private first — Scott and Ted as first users, building real products together as a business hub. May open to outside users over time. May stay private permanently. Both are valid outcomes.

### What the Current Framework Loses in This Model

Almost nothing of real value:
- **Iteration speed** — editing a markdown file today is instant; changing a skill in an application has more steps
- **Infrastructure simplicity** — the framework currently costs nothing to run; an application means hosting and maintenance

### What Improves Significantly

**Persistent project state** — the application holds the full picture of every active product continuously. No more reconstructing context from scratch each session. You open a session and it already knows what every product is, what phase it's in, what was last decided.

**Precise context passing** — today CLAUDE.md loads a large blob whether you're doing a bug fix or a full design sprint. The application assembles exactly the right context for the current task: design-review gets design identity + PRD + open slices, nothing else. Token budget goes entirely toward the work.

**Token efficiency** — two gains: (1) no session overhead re-loading context that was already established, (2) no noise in the context window. Sessions stay sharper longer. In a BYOK model this is a direct product quality differentiator for users.

**Multi-project visibility** — all active builds, their phases, their health, in one place. Currently requires visiting each project's files separately.

**Structural phase enforcement** — phase gates become structural, not instructional. The application cannot open a build phase if tech-context doesn't exist. Currently that's Claude following instructions — a weaker guarantee.

**Skill versioning and distribution** — skill improvements propagate across all active projects. Today they only apply to projects started after the change date.

**Solo Companion as first-class interface** — currently a Cloudflare Worker add-on. In the application, it's central.

**Collaboration** — Scott + Ted as co-builders on shared project state with clear ownership. True multi-user from day one.

**Scheduled tasks and automation** — owned by the application, not macOS LaunchAgent hacks.

### The Drift Problem (Addressed)

Concern: if the application only passes relevant context per phase, does Claude lose the full picture and start drifting?

Answer: the drift problem today is *worse* in the current model because it depends on Claude holding state across long sessions that compact. In the application model, Claude doesn't hold state — the application holds state and feeds it precisely when needed. The application never compacts, never forgets, never drifts. Claude operates on clean signal assembled from the authoritative source.

The application IS the brain. Claude is the reasoning engine for the current task. Splitting those jobs is a feature, not a vulnerability.

### The Post-Deploy Gap (Addressed)

The current framework has almost no story for live products. It ends at deploy. Bug-fix and enhancement are single-pass tools, not an ongoing operating model. There's no concept of a live product with users, growth, accumulated debt, or prioritization decisions across active versions.

The application holds the full product lifecycle — not just "build this slice" but "here are your five active products, here's the health of each, here's what's in flight, here's what's broken." The current framework becomes the foundation the application is built on, not a system to be replaced.

**What the application owns:** continuity layer, build history, product intelligence, collaboration state.
**What existing tools own:** production ops (Datadog, Sentry), task management (GitHub Issues, Linear), code (GitHub). Don't rebuild what those do well.

### The Defensible Moat

Anthropic will improve memory. Cursor will improve project context. Neither is trying to build an opinionated, process-enforcing, lifecycle-managing product layer for solo builders. That's not their business. The only way to be displaced is if someone builds the same product with the same process depth — which means replicating years of framework decisions, quality gates, design discipline, and continuity model. Copying skill files from GitHub gets them the prompts. It doesn't get them the product or the judgment behind it.

### Open Questions for Design Session

- What is the interface? Web app is the natural answer given Scott's track record and the ~/Apps context.
- Is the Solo Companion the seed of this product or a feature within it?
- How does skill versioning work — per-user override capability or framework-controlled canonical?
- What does the multi-project dashboard actually show? Phase, health, last activity, open decisions?
- How does the BYOK key management work — stored encrypted per user, or passed per session?
- Does this replace the engineering-playbook flat files or live alongside them during a transition period?

### Activation

**Codename: Han Solo** — private to Scott and Ren only. Never referenced in engineering-playbook or any shared artifact.

To open a design session: say "Han Solo" or "open Han Solo" directly to Ren. Ren reads ren-memory/shared/ideas.md to re-establish context before starting. Do NOT use "guided on" — that routes through the framework and loses this thread.

### Next Steps (defined 2026-05-05)

Four design sessions required before any build begins. In order:

1. **Module inventory** — formally name every existing module, what it owns, and what its interface is. Foundation for everything else.
2. **Application architecture** — how modules connect, how context is assembled and passed to Claude, BYOK key management, persistent project state data model.
3. **Interface design** — what the application looks like to use. Solo Companion as the seed.
4. **Collaboration model** — how Scott and Ted operate together. Ownership, visibility, permissions across shared and separate projects.
