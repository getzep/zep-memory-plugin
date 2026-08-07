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

The repository root conforms to
[Agent Plugins 1.0.0](https://agent-plugins.org/): `plugin.json` identifies the
portable package, `skills/` contains the shared skill, and `mcp.json` declares
the Memory MCP server using the standard `streamable-http` transport.

Claude and OpenAI compatibility does not depend on either vendor adopting that
standard:

- `.claude-plugin/plugin.json` and `.mcp.json` package the same components for
  Claude Desktop Chat / Cowork.
- `.codex-plugin/plugin.json` and `.mcp.json` package them for ChatGPT Work.

`.codex-plugin/` is the ChatGPT Work package format. Keeping that manifest does
not mean this plugin targets Codex as a coding product.

## Repository and distribution

This repository is the canonical source for one portable package with two
vendor wrappers. All paths load the same skill and point to the same Memory MCP
endpoint. The shared Zep marketplace in
[`getzep/zep`](https://github.com/getzep/zep) points here; no submodule or copied
package is required.

## Releasing

Keep the portable, Claude, and ChatGPT Work manifest versions synchronized:

```bash
python3 scripts/plugin_manifests.py set <version>
```

Run `claude plugin validate . --strict` and
`python3 scripts/plugin_manifests.py --check`, then run
`python3 scripts/validate_agent_plugin.py` before opening a PR.
