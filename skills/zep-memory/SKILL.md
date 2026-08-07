---
name: zep-memory
description: Use Zep as long-term memory for knowledge work in Claude (Desktop Chat / Cowork) and ChatGPT Work. Trigger when the user asks to remember something, recall prior context, search memory, work with user graphs or standalone graphs, or keep durable context across sessions.
---

# Zep Memory

Teach Claude / ChatGPT Work to use Zep as **agent memory for knowledge work** — not as a coding-agent / codebase-context product.

Use the `zep-memory` MCP server. The authenticated identity fixes the user and project; do not invent user IDs, project IDs, or graph selectors for the user's own memory tools.

## What Zep is here for

- Durable memory across sessions for the person using this client
- User graphs (personal memory, threads) and standalone graphs (shared / domain knowledge), when the connection allows them
- LLM-mediated writes via MCP tools (`add_memory`, and standalone write tools when available) — automatic message hooks are out of scope for this plugin

## Core ideas (expand later)

- **User graph** — memory for the signed-in person; search / summary / add tools target it implicitly
- **Threads** — conversation history that feeds the user graph (product model; MCP may expose message-shaped writes)
- **Standalone graphs** — project graphs beyond the user graph when enabled; list/search/add only with the standalone tools and a `graph_id`
- Prefer retrieving useful context before answering from guesswork; prefer writing durable facts the user wants remembered

## MCP tools (current Memory MCP)

- `search_graph` — search the user's memory (context block by default)
- `get_user_summary` — narrative summary of the user
- `add_memory` — add text / JSON / message to the user's memory when writes are allowed
- Optional standalone tools (`list_graphs`, `search_graph_in`, `add_memory_to_graph`) when that capability is enabled

Exact tool behavior and auth live in the Memory MCP docs / live tool schemas — prefer those over this skill if they disagree.

## Non-goals for this plugin

- Claude Code, Codex, Cursor, or other developer-tool positioning
- Auto-ingesting every chat turn via hooks
- Competing as a coding-context / repo-graph product
