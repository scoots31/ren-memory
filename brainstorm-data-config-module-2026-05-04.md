# Brainstorm: Data Config Module
**Date:** 2026-05-04
**Participants:** Scott + Ren
**Status:** Concept stage — ready for discovery

---

## Origin

Started as a conversation about what companies encounter when switching software or processes. Scott shared his background as an implementation consultant — embedded in customer companies, learning their processes end to end before implementing software. The consistent failure point: when discovery exposed deep process dysfunction, leadership recoiled and blamed the software rather than owning the problems surfaced.

The key insight that opened the product conversation: **any product that introduces change will hit the same iceberg.** The framework teaches how to build the product. It doesn't address what happens when the product lands in the real world.

---

## The Core Problem

Product builders never think about how a current customer will migrate their existing data into the new product during the design phase. The answer is always "that's the customer's problem." That's wrong — and it's a primary reason products fail on adoption.

**The on-ramp is a product problem, not a customer problem.**

If getting existing data into the new product is hard, painful, or requires outside help:
- Customers keep one foot in the old system
- The transition never fully completes
- Shadow systems form
- Eventually the customer churns and says the product "didn't work"

The product didn't fail. The on-ramp failed. And by the time that's visible, the product team has moved on and blames retention numbers on something else.

---

## The Three Problems at the Heart of It

1. **Old data rarely maps cleanly to the new structure** — inconsistency surfaces at the worst possible moment
2. **Migrate vs. archive vs. abandon decisions are harder than anyone expects** — no instrument exists to guide them
3. **Parallel running creates double work and source-of-truth confusion** — because nobody is confident the migration is complete or correct

These three are a sequence. You can't make good migration decisions without an honest picture of the data. You can't confidently cut to one source of truth until decisions are made. Parallel chaos is a symptom of the first two being unresolved.

**The real problem the tool solves is confidence.**

---

## The Product Concept: Data Config Module

### Core Architecture

```
Product Code → Data Config Module → Any Database
```

An abstraction layer that sits between the product and the database. The product doesn't know or care what's underneath. The module knows both the product's data language and the target database's language — and handles translation between them.

### What It Does

**Step 1 — Read both ends**
- Read what the product is producing: fields, data types, relationships, volume
- Read what the database expects: schema, constraints, what it can and can't accept
- Produce an honest gap report: green (maps cleanly), yellow (needs a decision), red (will break things)
- In plain language — not a technical error log. A business person can read and act on it.

**Step 2 — Human decision layer**
- Someone with business context walks through the yellows and reds
- Defines every translation rule — what old field maps to what new field, how values transform
- Those rules become the documented translation logic living in the module
- This documentation is a valuable artifact — a clear record of every mapping decision made

### Four Operating Modes (three viable for MVP)

**Router** — product data comes in, module sends it to configured destination(s). Simplest mode. Gets the module in place and proves the concept.

**Chain** *(recommended MVP)* — product writes to existing database as always. Module watches and replicates to new database asynchronously. Least invasive — existing system untouched, nothing breaks. Lowest barrier to customer adoption. Seeds validation capability as a natural byproduct.

**Splitter** — parallel write to both old and new database simultaneously. Both stay current. More powerful during active transition but requires more customer trust — data is intercepted before hitting existing database.

**Validator** *(deferred — non-trivial)* — continuously compares both databases, confirms mapping is correct, flags discrepancies. Hard because it's comparing meaning not just structure. Chain mode partially seeds this capability.

### Key Design Principle

The module is not a straight copy — it's a translation layer. "player_status" with values 1, 2, 3 in the old schema becomes "roster_designation" with values "active", "injured", "cut" in the new one. The translation rules are what lives in the module and what gets configured before any data moves.

---

## What Already Exists (and Why It's Not the Answer)

- **ETL tools** (Informatica, Talend, Apache NiFi) — powerful but built for data engineers. Complex, expensive, require significant technical expertise.
- **Database migration tools** (AWS DMS, Flyway, Liquibase) — focused on same-type database moves, not business logic translation.
- **iPaaS platforms** (MuleSoft, Boomi, Zapier) — expensive at enterprise level, shallow at consumer level.

**None of these approach the problem the way this does.** They assume technical users. They assume mapping decisions are already made. They don't surface gaps in plain language. They don't guide a business person through translation decisions. They don't live at the product design phase.

### The Real Differentiation

**Not reinventing the tool landscape — inventing a process to use them.**

The technology is solvable with open source tooling (SQLAlchemy, Pandas, standard database connectors). The value is the intelligence layer: the workflow, the decision framework, the plain language interface, the sequencing from "messy data in old system" to "validated translation layer and confident cutover plan."

The tools are the engine. This is the vehicle.

Scott's moat: domain knowledge from years of embedded implementation work. Nobody can replicate knowing exactly where the process breaks down without living it.

---

## Build Philosophy

- **Open source under the hood** — invisible infrastructure the customer never manages
- **No expensive dependencies** — no licenses, no external setup, no separate learning curves
- **One thing the customer interacts with** — the Data Config Module. Everything else is underneath.
- **Avoid cobbling** — if using it requires managing multiple expensive tools, the product has become the iceberg

---

## Target Market

**Small and medium size companies.**

Enterprise companies have the people and resources to solve this already. SMBs are the underserved gap:
- Too big to manually move data themselves
- Too small to afford enterprise migration tooling or dedicated data teams
- Abandoned by vendors after the sale

Sales motion: selling to owners and ops directors who lived the pain firsthand. They recognize the problem in five minutes. The story sells itself.

Pricing: accessible, not enterprise contract territory. Cost can't be its own iceberg.

---

## Dual Lifecycle

The module serves two distinct moments:

1. **During product design** — forces the builder to think through the data on-ramp before the model is locked. What will customers bring? What are the common source formats? What's the minimum viable data shape for day one?

2. **During implementation** — the instrument the implementer uses to bring a customer's actual data in cleanly, using the translation rules defined during design.

Work done in moment one directly reduces pain in moment two. For a solo builder who is both the product designer and the implementer, one coherent tool spanning both moments is exactly what's needed.

---

## Open Questions / Next Steps

- What's the MVP scope — chain mode first, with router as the simpler predecessor?
- What databases does v1 support? (Postgres, SQLite, MySQL as the obvious starting three)
- What does the gap report actually look like as a user interface?
- How does a non-technical user configure translation rules — guided form, visual mapper, something else?
- Does this live as a standalone product or as a module within the framework's build process?
- Pricing model — per project, subscription, per company size?
