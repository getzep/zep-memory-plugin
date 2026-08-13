# Zep Memory

Knowledge-work plugin for **Claude Desktop Chat**, **Claude Cowork**, and
**ChatGPT Work**. Not positioned for Claude Code, Codex, Cursor, or other
developer-tool surfaces.

Bundles:

- The `zep-memory` skill — how to use Zep for user memory, threads, and
  standalone graphs via LLM-mediated Memory MCP tool calls
- The Zep Memory MCP server at `https://api.getzep.com/mcp`

## Product intent

- Teach knowledge-work assistants to use Zep as durable memory
- LLM-written memories via MCP tools are the v1 write path (hooks /
  auto-ingest of every message are out of scope)
- Internal rollout first via Claude Team / Enterprise and ChatGPT workspace
  distribution; public marketplace later
- Docs follow-up: install instructions on the Memory MCP docs page

## Packaging note

`plugins/zep-memory/` is the **one plugin root** (ChatGPT cannot install a
marketplace `source.path` of `"./"`). That directory conforms to
[Agent Plugins 1.0.0](https://agent-plugins.org/): `plugin.json` identifies the
portable package, `skills/` contains the skill, and `mcp.json` declares
the Memory MCP server using the standard `streamable-http` transport.

Claude and OpenAI compatibility does not depend on either vendor adopting that
standard. The same folder also holds `.claude-plugin/`, `.codex-plugin/`, and
`.mcp.json`.

`.codex-plugin/` is the ChatGPT Work package format. Keeping that manifest does
not mean this plugin targets Codex as a coding product.

## Repository and distribution

This repository is the catalog plus that one package. Both marketplace
entries point at `./plugins/zep-memory`.

```bash
claude plugin marketplace add getzep/zep-memory-plugin
claude plugin install zep-memory@zep-memory
```


## Releasing

Keep portable, Claude, and ChatGPT Work manifest versions synchronized with
`python3 scripts/plugin_manifests.py set <version>`, validate, then open a PR.
How that reaches users (Claude org marketplace, ChatGPT Work workspace share,
public directories later) is documented under **Releasing** in [`AGENTS.md`](AGENTS.md).
