# Changelog

All notable changes to the Zep Memory plugin.

## Unreleased

- Host Claude and ChatGPT Work marketplace catalogs in this repository with
  same-repo plugin sources. Install with
  `claude plugin marketplace add getzep/zep-memory-plugin` then
  `claude plugin install zep-memory@zep-memory`.

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
