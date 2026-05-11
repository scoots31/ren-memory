# Anthropic Release Notes — Framework Scan

**last_reviewed:** 2026-05-11
**scan_run:** 2026-05-11
**framework_version_at_scan:** v2.7.0

---

## New Items — 2026-05-11 scan

One dated entry since last scan (2026-05-05): May 6, 2026. Four sub-items, all in the Managed Agents track. These formally confirm the public beta status of capabilities Scott already logged from X/social on 2026-05-07 (see context.md "Managed Agents — Han Solo Infrastructure Layer"). Treated here from the perspective of framework impact.

### Act

None this week.

### Watch

- **Multiagent sessions + Outcomes — public beta (2026-05-06)** — Already mapped to Han Solo as the orchestration and quality-contract layers. Scott's call: when to start a Han Solo design-build session that uses these capabilities directly vs continuing to use Claude Code's local Agent tool inside the framework. No framework skill changes needed today.
- **Webhooks for Managed Agents — supported (2026-05-06)** — Already identified as the path to event-driven Solo Companion updates (vs current polling). Scott's call: schedule a Solo Companion architecture session to spec the migration off polling, or defer until Han Solo runtime is in place.

### Skip

- **Vault credential background refresh for mcp_oauth (2026-05-06)** — No current framework integration with Managed Agents vault; affects only consumers building on the Managed Agents runtime.
- **Filtering/sorting for sessions and events (2026-05-06)** — API quality-of-life on Managed Agents endpoints; no framework consumer today.

---

## Previous Scans

- **2026-05-11** — framework v2.7.0 — 0 Act / 2 Watch / 2 Skip — May 6 Managed Agents release: Multiagent + Outcomes + Webhooks public beta confirmed; already absorbed into Han Solo planning context.
- **2026-05-05** — framework v2.3.0 — 0 Act / 0 Watch / 0 Skip — initialization scan, no new items past seed date.
