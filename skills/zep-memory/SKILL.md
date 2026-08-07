---
name: zep-memory
description: Use Zep as long-term memory for knowledge work. Use when lasting preferences, corrections, procedural how-you-work facts, or decisions should stick across sessions; when recalling prior context for related work; or when searching or updating user or standalone graphs.
---

# Zep Memory

Use the `zep-memory` MCP server for durable memory across sessions. Auth fixes the user and project — do not invent user IDs, project IDs, or graph selectors for the user's own memory tools.

## When to act

- **Read** before guessing: search or summarize when prior context would improve the answer.
- **Write** durable facts without waiting for "remember this": preferences, corrections, procedures, and decisions that should persist. Skip ephemeral chat noise.
- LLM-mediated MCP writes only — do not assume every turn is auto-ingested.

## Tools

- `search_graph` — search user memory
- `get_user_summary` — narrative user summary
- `add_memory` — write to user memory (when allowed)
- Optional standalone tools (`list_graphs`, `search_graph_in`, `add_memory_to_graph`) when enabled — require a `graph_id`

Prefer live MCP tool schemas over this skill if they disagree.
