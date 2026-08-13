# Zep Memory plugin maintainer instructions

This repository root is one Agent Plugins 1.0.0 package with compatibility
wrappers for Claude Desktop Chat / Cowork and ChatGPT Work.

## Package architecture

- Keep `skills/zep-memory/` as the canonical skill (Agent Plugins + Claude).
- Materialize a **real** copy at `plugins/zep-memory/skills/` for ChatGPT Work.
  Do not use a symlink — Codex/ChatGPT silently drop outbound symlinks when
  installing (openai/codex#24770), which ships a plugin with MCP config and
  no skill. After editing the canonical skill, run
  `python3 scripts/plugin_manifests.py sync-skills` (also runs on `set`).
  `--check` fails if the copy is missing, a symlink, or drifted.
- Keep the plugin name `zep-memory` and version identical in:
  - `plugin.json`
  - `.claude-plugin/plugin.json`
  - `plugins/zep-memory/.codex-plugin/plugin.json`
- Keep root `plugin.json` and `mcp.json` conformant to Agent Plugins 1.0.0.
- Preserve `.claude-plugin/plugin.json` and root `.mcp.json`; Claude support
  must not depend on Claude adopting the portable format.
- Keep the ChatGPT Work package under `plugins/zep-memory/`
  (`.codex-plugin/plugin.json`, `.mcp.json`, and a real `skills/` directory).
  OpenAI support must not depend on OpenAI adopting the portable format.
- `.codex-plugin/` is the package format used by ChatGPT Work; this plugin is not
  positioned for Codex coding workflows.
- Keep every `.mcp.json` and `mcp.json` pointed at the production Zep Memory MCP
  endpoint `https://api.getzep.com/mcp` (HTTP / `streamable-http` as required).
- Do not add Cursor packaging unless the product scope explicitly expands.
- This repository hosts its own marketplace catalogs
  (`.claude-plugin/marketplace.json` and `.agents/plugins/marketplace.json`).
  Plugin `source` entries must stay same-repo paths so install does not require
  a second GitHub clone: Claude uses `./`; ChatGPT Work uses
  `./plugins/zep-memory` (OpenAI rejects local `source.path` of `"./"`).

## Releasing

### Repo ritual (always)

For runtime content changes (skill, manifests, MCP package config):

1. Run `python3 scripts/plugin_manifests.py set <version>`.
2. Add a `CHANGELOG.md` entry.
3. Validate the Claude plugin package and the marketplace catalog separately.
   With both files under `.claude-plugin/`, `claude plugin validate .` only
   checks the marketplace:

   ```bash
   claude plugin validate .claude-plugin/plugin.json --strict
   claude plugin validate . --strict
   ```

4. Run `python3 scripts/plugin_manifests.py --check` (includes marketplace
   same-repo source checks).
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

#### This repository as Claude marketplace

```bash
claude plugin marketplace add getzep/zep-memory-plugin
claude plugin install zep-memory@zep-memory
```

Marketplace entries must stay free of `version`; the host resolves the release
from this package's manifests. Ordinary plugin releases do not need a separate
marketplace PR elsewhere. This is still not Anthropic's official marketplace.

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
  or git-backed marketplace sources): the entry's `source.path` must be
  `./plugins/zep-memory` (not `"./"`). Update the nested package or marketplace
  source, then restart / refresh so the host reloads. OpenAI caches installed
  copies by marketplace + name + version.

#### OpenAI universal public Plugins Directory (later)

Official public listing. Submit through the OpenAI plugin submission portal
(review → Publish). Updates require a new draft with a **new** manifest
`version`, release notes, resubmit, approval, then Publish again. Merging to
GitHub does not update the live directory listing by itself. Skills may be
snapshotted at review time; live MCP tool calls still hit the production
Memory MCP endpoint.
