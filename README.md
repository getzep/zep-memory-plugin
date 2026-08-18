# Zep Memory

Knowledge-work plugin for **Claude Desktop Chat**, **Claude Cowork**,
**ChatGPT Work**, and **Cursor**. Not positioned for Claude Code, Codex, or
other coding-agent CLIs.

Bundles:

- The `zep-memory` skill — how to use Zep for user memory, threads, and
  standalone graphs via LLM-mediated Memory MCP tool calls
- The Zep Memory MCP server at `https://api.getzep.com/mcp`

## Terms and privacy

**Read these before installing or using this plugin.** They are the terms of
service and privacy policy for this plugin.

- **[Terms of Service](https://www.getzep.com/legal/terms/)**
- **[Privacy Policy](https://www.getzep.com/legal/privacy/)**

The plugin files (skill, manifests, and MCP config) are also licensed under the
[Apache License 2.0](LICENSE).

This plugin connects the host to Zep’s Memory MCP server. You sign in with
OAuth. After that, the assistant can search and add to your Zep memory — your
user graph, and standalone graphs when your project enables them. That API and
memory data is handled under the Privacy Policy above.

## Usage

Once installed and signed in, ask the assistant to recall or save durable
context. For example:

- What do you remember about me?
- Search my Zep memory for relevant context on this topic.
- Save this to my Zep memory.

The skill tells the assistant to prefer Zep over the host’s built-in memory,
to look up stored context before guessing, and to write durable facts
(preferences, corrections, procedures, decisions, feedback) without waiting to
be asked.

Install and product docs: [Memory MCP server](https://help.getzep.com/memory-mcp-server).

## Configuration

No API keys, plugin variables, or other secrets belong in this repository. Auth
is client-managed OAuth against `https://api.getzep.com/mcp`.

The first Memory MCP call should prompt for Zep sign-in (work email → identity
provider → project). You need a Zep account whose project has Memory MCP
enabled.

## Support

- Docs: [Memory MCP server](https://help.getzep.com/memory-mcp-server)
- Issues: [getzep/zep-memory-plugin](https://github.com/getzep/zep-memory-plugin/issues)

## Product intent

- Teach knowledge-work assistants to use Zep as durable memory
- LLM-written memories via MCP tools are the v1 write path (hooks /
  auto-ingest of every message are out of scope)
- Internal rollout via Claude Team / Enterprise, ChatGPT workspace
  distribution, and Cursor local / team-marketplace install; public Cursor
  Marketplace listing is a separate Cursor review after submitting this
  repository
- Docs follow-up: install instructions on the Memory MCP docs page

## Packaging note

`plugins/zep-memory/` is the **one plugin root** (ChatGPT cannot install a
marketplace `source.path` of `"./"`). That directory conforms to
[Agent Plugins 1.0.0](https://agent-plugins.org/): `plugin.json` identifies the
portable package, `skills/` contains the skill, and `mcp.json` declares
the Memory MCP server using the standard `streamable-http` transport.

Cursor loads that Agent Plugins package as-is. Claude and OpenAI compatibility
does not depend on either vendor adopting the standard. The same folder also
holds `.claude-plugin/`, `.codex-plugin/`, and `.mcp.json`.

`.codex-plugin/` is the ChatGPT Work package format. Keeping that manifest does
not mean this plugin targets Codex as a coding product.

## Repository and distribution

This repository is the catalog plus that one package. Marketplace entries
point at `./plugins/zep-memory` (Claude and ChatGPT Work) or `plugins/zep-memory`
(Cursor, via `pluginRoot`).

```bash
claude plugin marketplace add getzep/zep-memory-plugin
claude plugin install zep-memory@zep-memory
```

### Cursor (local test)

Symlink the plugin root — not this repository root — into Cursor's local
plugin directory, then reload:

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /absolute/path/to/zep-memory-plugin/plugins/zep-memory ~/.cursor/plugins/local/zep-memory
```

Then in Cursor: **Developer: Reload Window**. Open **Customize** and confirm
the `zep-memory` skill and MCP server are present. The first Memory MCP call
should prompt for Zep OAuth (work email → IdP → project).

You can also import this GitHub repository as a Cursor team marketplace;
`.cursor-plugin/marketplace.json` points at the same plugin root.

To list on the public [Cursor Marketplace](https://cursor.com/marketplace),
submit this repository at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).
Cursor reviews the listing and every later update; merging to GitHub does not
publish by itself.

## Releasing

Keep portable, Claude, and ChatGPT Work manifest versions synchronized with
`python3 scripts/plugin_manifests.py set <version>`, validate, then open a PR.
How that reaches users (Claude org marketplace, ChatGPT Work workspace share,
Cursor team marketplace, public Cursor Marketplace) is documented under
**Releasing** in [`AGENTS.md`](AGENTS.md).
