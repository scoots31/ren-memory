# Han Solo MCP — Setup Guide

**Last updated:** 2026-05-14
**Purpose:** Step-by-step instructions for wiring the Han Solo MCP into Claude Code Desktop on any machine. Written for Ted's setup but applies to any new builder.

---

## What this does

The Han Solo MCP connects Claude Code to Ren's memory layer (Letta on Render). Once configured, 15 tools become available in every Claude Code session — reading/writing signals, session briefs, portraits, project state, and more.

---

## What you need before starting

- Claude Code Desktop installed and running
- Your Han Solo bearer token (get from Scott — each user has their own)
- Mac with internet access

---

## The one command that matters

Open a terminal and run:

```bash
"/Users/<your-username>/Library/Application Support/Claude/claude-code/2.1.138/claude.app/Contents/MacOS/claude" mcp add --transport http --scope user han-solo https://han-solo-mcp.onrender.com/mcp --header "Authorization: Bearer <YOUR_TOKEN>"
```

Replace `<your-username>` with your Mac username and `<YOUR_TOKEN>` with your bearer token.

**For Ted:** token is `eVq0eGBoX1rGNatZyaDw8yYW0l4bZ8viGmyxsN1Y8GA`

So Ted's command is:

```bash
"/Users/<ted-username>/Library/Application Support/Claude/claude-code/2.1.138/claude.app/Contents/MacOS/claude" mcp add --transport http --scope user han-solo https://han-solo-mcp.onrender.com/mcp --header "Authorization: Bearer eVq0eGBoX1rGNatZyaDw8yYW0l4bZ8viGmyxsN1Y8GA"
```

---

## What that command does

It writes an entry to `~/.claude.json` (user-level MCP config) that looks like this:

```json
"han-solo": {
  "type": "http",
  "url": "https://han-solo-mcp.onrender.com/mcp",
  "headers": {
    "Authorization": "Bearer <YOUR_TOKEN>"
  }
}
```

---

## After running the command

1. Fully quit Claude Code Desktop (Cmd+Q — not just close the window)
2. Reopen it
3. Start a new session
4. The han-solo tools should appear in the available tools list

---

## How to verify it worked

In a Claude Code session, ask Claude to use `get_session_brief` or `read_core_memory`. If it responds with real content, the MCP is connected.

Or check the logs: `~/Library/Logs/Claude/main.log` — look for a line with `Calling SDK with N total servers` where N is one more than before.

---

## Things we learned the hard way (do not repeat)

1. **Do NOT add han-solo to `~/.claude/settings.json`** — that file is for the Claude Code CLI, not the Desktop app. The Desktop app silently ignores URL-type MCPs in settings.json.

2. **Do NOT add han-solo to `~/Library/Application Support/Claude/claude_desktop_config.json`** — that file only supports command-type (stdio) MCPs. URL-type entries get logged as "invalid" and skipped.

3. **The `type` field is required** — `"type": "http"` (or `"streamable-http"`) must be present or the entry is silently ignored.

4. **`~/.claude.json` is the right file**, set via `claude mcp add --scope user`. This is what the Desktop app reads for URL-type MCPs.

5. **The Claude binary path may change with version updates.** The path `claude-code/2.1.138/claude.app/Contents/MacOS/claude` includes the version number. If the command fails, check `~/Library/Application Support/Claude/claude-code/` for the current version folder.

---

## Claude Code version check

To find the current claude binary path:

```bash
ls ~/Library/Application\ Support/Claude/claude-code/
```

Use the highest version number folder.

---

## Scope of access

Each token has different permissions:

| User | Token | Role | Access |
|------|-------|------|--------|
| Scott | `RHcpXjeAJlu_DzhYplsLaUOUSGVrU-gceamJQoXb81Q` | Owner | Full — all visibility tiers |
| Ted | `eVq0eGBoX1rGNatZyaDw8yYW0l4bZ8viGmyxsN1Y8GA` | Collaborator | Shared + client-joint only |

New users require a token to be added to the Render service env vars by Scott before the MCP will accept their requests.
