# Zep Memory

Use Zep as long-term memory for knowledge work in **Cursor**, **Claude Desktop
Chat**, **Claude Cowork**, and **ChatGPT Work**.

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
[Apache License 2.0](../../LICENSE).

This plugin connects Cursor (and other supported hosts) to Zep’s Memory MCP
server. You sign in with OAuth. After that, the assistant can search and add to
your Zep memory — your user graph, and standalone graphs when your project
enables them. That API and memory data is handled under the Privacy Policy
above.

## Usage

Once installed and signed in, ask the assistant to recall or save durable
context. For example:

- What do you remember about me?
- Search my Zep memory for relevant context on this topic.
- Save this to my Zep memory.

The skill tells the assistant to prefer Zep over the host’s built-in memory,
to look up stored context before guessing, and to write durable facts (preferences,
corrections, procedures, decisions, feedback) without waiting to be asked.

Install and product docs: [Memory MCP server](https://help.getzep.com/memory-mcp-server).

## Configuration

No API keys, plugin variables, or other secrets belong in this package. Auth is
client-managed OAuth against `https://api.getzep.com/mcp`.

The first Memory MCP call should prompt for Zep sign-in (work email → identity
provider → project). You need a Zep account whose project has Memory MCP
enabled.

## Support

- Docs: [Memory MCP server](https://help.getzep.com/memory-mcp-server)
- Issues: [getzep/zep-memory-plugin](https://github.com/getzep/zep-memory-plugin/issues)
