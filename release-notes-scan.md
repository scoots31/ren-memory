# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-05-25
**scan_run:** 2026-05-25
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-05-25 scan

Four entries since last scan (2026-05-18), all dated 2026-05-19 and all in the Managed Agents family.

### Act

None this week.

### Watch

- **MCP tunnels — Research Preview (2026-05-19)** — Lets agents connect to MCP servers running inside a private network. Not relevant to the framework today (Han Solo MCP is a public bearer-token endpoint), but Scott's call on the Han Solo NDA-tier roadmap: if a client ever requires their MCP servers to stay inside their own network, tunnels would be the path.
- **Self-hosted sandboxes for Claude Managed Agents (2026-05-19)** — Run Managed Agents tool execution on the customer's own infrastructure instead of Anthropic's. Han Solo is built on Letta, not Managed Agents — but the curator notes flag Managed Agents (specifically Dreaming and Outcomes) as candidates for future integration. Scott's call: if Han Solo ever leans on Managed Agents for regulated/NDA clients, self-hosted sandboxes are the data-privacy unlock.

### Skip

- **Update MCP config mid-session for Managed Agents (2026-05-19)** — Convenience for live Managed Agents sessions. Framework does not run on Managed Agents, so no skill consumes this.
- **Large output auto-spill to file for Managed Agents (2026-05-19)** — Tool outputs >100K tokens auto-write to sandbox file with truncated preview. Useful for Managed Agents harnesses, but Han Solo's Letta-based runtime handles long outputs through its own memory layer.

---

## Previous Scans

- **2026-05-18** — framework v2.7.0 — 0 Act / 1 Watch / 0 Skip — Fast mode opened to Opus 4.7 via API beta header (separate from Claude Code `/fast` toggle).
- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
