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
