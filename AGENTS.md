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

### Repo ritual (always)

For runtime content changes (skill, manifests, MCP package config):

1. Run `python3 scripts/plugin_manifests.py set <version>`.
2. Add a `CHANGELOG.md` entry.
3. Run `claude plugin validate . --strict`.
4. Run `python3 scripts/plugin_manifests.py --check`.
5. Run `python3 scripts/validate_agent_plugin.py` and confirm
   `.github/workflows/test-plugin.yml` passes.
6. Open a PR and merge to the default branch.

There is no npm publish, GitHub Release, or required git tag for this package.
The Memory MCP server at `https://api.getzep.com/mcp` ships separately —
server-only changes do not require a plugin version bump unless package
metadata or the skill change too.

Bump the synced `version` fields whenever you want installed clients to treat
the package as new. With an explicit version set, hosts often cache by that
string and skip updates if it is unchanged.

### Distribution channels

How a merge reaches users depends on the channel. This repo is the source of
truth; channels differ in what happens after merge.

#### Claude Team / Enterprise org marketplace (primary internal path)

Not Anthropic's official curated marketplace. Org owners distribute via a
manual ZIP marketplace and/or a private GitHub-synced marketplace.

- **GitHub-synced marketplace:** connect a private repo (this plugin repo, or a
  marketplace catalog that points at it). Manual **Update** always works.
  Optional **Sync automatically** runs only when a PR that includes a plugin
  **version bump** is merged to the default branch — direct pushes do not
  auto-sync. Syncs can take up to ~30 minutes.
- **Manual ZIP upload:** uploading a package with the same plugin name
  overwrites the previous org copy; no GitHub merge required for that path.

#### Custom / Zep-hosted Claude marketplace (`getzep/zep`)

The shared marketplace in [`getzep/zep`](https://github.com/getzep/zep) points
at this repository (no submodule or copied package). Users refresh marketplace
data (or rely on marketplace auto-update) to see new versions. Catalog or
source-ref edits in `getzep/zep` are only needed when the marketplace entry
itself changes — not for ordinary plugin version bumps here. This is still
not Anthropic's official marketplace.

#### Anthropic official marketplace (later)

Curated by Anthropic; inclusion and updates are not "merge this repo and it
ships." Treat as a separate submission / review process when public listing
is in scope.

#### ChatGPT Work — workspace share and local / repo marketplaces (internal)

Not the universal public Plugins Directory.

- **Workspace share:** install/test from the ChatGPT desktop app (Work), then
  Share to members or groups. GitHub merge alone does not propagate the
  package; re-share or refresh the local install as needed.
- **Local / repo / personal marketplaces** (`.agents/plugins/marketplace.json`
  or git-backed marketplace sources): update the plugin files or marketplace
  source, then restart / refresh so the host reloads the package. OpenAI
  caches installed copies by marketplace + name + version.

#### OpenAI universal public Plugins Directory (later)

Official public listing. Submit through the OpenAI plugin submission portal
(review → Publish). Updates require a new draft with a **new** manifest
`version`, release notes, resubmit, approval, then Publish again. Merging to
GitHub does not update the live directory listing by itself. Skills may be
snapshotted at review time; live MCP tool calls still hit the production
Memory MCP endpoint.
