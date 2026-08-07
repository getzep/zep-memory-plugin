# Zep Memory plugin maintainer instructions

This repository root is one Agent Plugins 1.0.0 package with compatibility
wrappers for Claude Desktop Chat / Cowork and ChatGPT Work.

## Package architecture

- Keep one shared `skills/zep-memory/` tree; do not create vendor-specific copies.
- Keep the plugin name `zep-memory` and version identical in:
  - `plugin.json`
  - `.claude-plugin/plugin.json`
  - `.codex-plugin/plugin.json`
- Keep root `plugin.json` and `mcp.json` conformant to Agent Plugins 1.0.0.
- Preserve `.claude-plugin/plugin.json` and `.mcp.json`; Claude support must not
  depend on Claude adopting the portable format.
- Preserve `.codex-plugin/plugin.json` and `.mcp.json`; OpenAI support must not
  depend on OpenAI adopting the portable format.
- `.codex-plugin/` is the package format used by ChatGPT Work; this plugin is not
  positioned for Codex coding workflows.
- Keep `.mcp.json` pointed at the production Zep Memory MCP endpoint:
  `https://api.getzep.com/mcp` using remote HTTP transport.
- Keep `mcp.json` pointed at that endpoint using the Agent Plugins
  `streamable-http` transport and canonical 1.0.0 schema.
- Do not add Cursor packaging unless the product scope explicitly expands.
- The shared marketplace remains in `getzep/zep` and points at this repository.
  Do not add submodules or duplicate the plugin back into that marketplace repo.

## Releasing

For runtime content changes:

1. Run `python3 scripts/plugin_manifests.py set <version>`.
2. Add a `CHANGELOG.md` entry.
3. Run `claude plugin validate . --strict`.
4. Run `python3 scripts/plugin_manifests.py --check`.
5. Run `python3 scripts/validate_agent_plugin.py` and confirm
   `.github/workflows/test-plugin.yml` passes.

There is no package-publish or tag step.
