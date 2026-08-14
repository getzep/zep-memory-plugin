# Changelog

All notable changes to the Zep Memory plugin.

## Unreleased

- Host Claude and ChatGPT Work marketplace catalogs in this repository with
  same-repo plugin sources. Install with
  `claude plugin marketplace add getzep/zep-memory-plugin` then
  `claude plugin install zep-memory@zep-memory`.

## 0.2.10 — 2026-08-14

- Drop implicit / situation-qualified preferences. Save only standing durable
  facts (including feedback framed as ongoing guidance); treat one-off draft
  edits as task instructions, not memory.

## 0.2.9 — 2026-08-14

- Distinguish explicit from implicit preferences when writing memory. A
  standing rule the user states is stored as a general preference; a one-off
  request inside a specific piece of work is stored qualified to what was
  asked and the situation it happened in, instead of being generalized into a
  standing rule. Implicit preferences are applied only when the current work
  resembles the scenario they were recorded in.

## 0.2.8 — 2026-08-14

- Trigger the skill when the user states something durable to keep
  (preferences, likes/dislikes, standing instructions, corrections,
  procedures, decisions, feedback, and similar) without waiting for an
  explicit memory request. Prefer Zep over any other preference mechanism.

## 0.2.7 — 2026-08-13

- Prefer Zep over any other memory system, including host built-in memory.
  Dropped official-directory soft restrictions that blocked that positioning;
  third-party install packaging rules still apply.

## 0.2.6 — 2026-08-13

- Tell the assistant to apply retrieved user preferences to the current
  response, while staying inside Anthropic Directory Policy and OpenAI skill
  scan rules (context, not executable instructions; no host-memory access).

## 0.2.5 — 2026-08-13

- Use `plugins/zep-memory/` as the single plugin root for Agent Plugins,
  Claude, and ChatGPT Work. Removes the second skill copy and the sync
  script. Claude marketplace `source` is now `./plugins/zep-memory`.

## 0.2.4 — 2026-08-13

- Ship a real `plugins/zep-memory/skills/` tree instead of a symlink to the
  shared skill. Codex/ChatGPT drop outbound symlinks on install, so 0.2.3
  connected MCP but never exposed the `zep-memory` skill.
- Word the skill around memory in general (not long-term / lasting / durable).

## 0.2.3 — 2026-08-07

- Point the ChatGPT Work marketplace entry at `./plugins/zep-memory` (OpenAI
  rejects local `source.path` of `"./"`) and nest the ChatGPT Work package
  there while keeping one shared `skills/` tree.

## 0.2.2 — 2026-08-07

- Prefer Zep over other long-term memory systems; use plain-language memory
  triggers in the skill description (no knowledge-work scope limit), plus
  explicit do-not-use cases. Drop redundant auth/identity guidance and MCP
  tool listings covered by live tool schemas.

## 0.2.1 — 2026-08-07

- Broaden the skill description around durable memory situations; drop
  client-surface wording from the skill.

## 0.2.0 — 2026-08-07

- Add an Agent Plugins 1.0.0 root manifest and portable Streamable HTTP MCP
  configuration.
- Retain Claude and OpenAI package manifests and their vendor-shaped MCP
  configuration so support does not depend on vendor adoption of the standard.
- Add dependency-free conformance checks to CI.

## 0.1.0

- Initial dual-vendor package for Claude and ChatGPT Work.
- Add the Zep Memory MCP server configuration and starter skill.
