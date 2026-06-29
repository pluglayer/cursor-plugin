# PlugLayer Cursor Plugin

PlugLayer's Cursor plugin is meant to feel like a sharp deployment teammate inside Cursor, not just an MCP connection.

## One-line install

```bash
curl -fsSL https://raw.githubusercontent.com/pluglayer/cursor-plugin/main/install.sh | bash
```

The installer gives the user a branded PlugLayer terminal flow, installs the plugin into Cursor's global local-plugin directory, asks for a token from [portal.pluglayer.com/tokens](https://portal.pluglayer.com/tokens), and supports later reinstall or token refresh without unnecessary re-entry.

It bundles:
- PlugLayer MCP access
- a deployment-aware Cursor rule
- focused skills
- focused agents for deploy, CI/CD, domains, and troubleshooting

The structure follows Cursor's plugin model:
- `.cursor-plugin/plugin.json`
- `mcp.json`
- `rules/*.mdc`
- `skills/*/SKILL.md`
- `agents/*.md`

References:
- [Cursor plugin reference](https://cursor.com/docs/reference/plugins)
- [Cursor official plugins repo](https://github.com/cursor/plugins)

## What this plugin gives the user

### MCP
Cursor can talk to PlugLayer for:
- projects
- apps
- databases / Data Layer
- domains
- deploy tasks
- CI/CD workflow generation

### Skills
The plugin includes practical skills for:
- repo inspection
- deploy
- custom domain setup
- deploy troubleshooting
- GitHub CI/CD setup

### Agents
The plugin includes focused agents so users can ask for specialized help without the general agent re-deriving the whole workflow every time:
- `pluglayer-deploy`
- `pluglayer-setup-cicd`
- `pluglayer-fix-deploy`
- `pluglayer-domain-setup`

## Install

### Requirements
1. Cursor with plugin support
2. `uvx` available on the machine that runs Cursor
3. A PlugLayer API token from [portal.pluglayer.com/tokens](https://portal.pluglayer.com/tokens)

### Installer behavior

- Installs the plugin into `~/.cursor/plugins/local/pluglayer-cursor-plugin`
- Creates a `cursor-pluglayer` launcher in `~/.local/bin`
- Prompts for a PlugLayer token during install
- Wires the PlugLayer MCP server into Cursor
- Detects the installed plugin version and offers:
  - update/reinstall PlugLayer for Cursor
  - update the saved token only

### Recommended local install
Using a symlink makes updates much nicer than copying the folder every time.

1. Create Cursor's local plugin directory if needed:

```bash
mkdir -p ~/.cursor/plugins/local
```

2. From the repo root, symlink this plugin into Cursor's local plugin directory:

```bash
ln -s "$(pwd)" ~/.cursor/plugins/local/pluglayer-cursor-plugin
```

3. Export your PlugLayer API token before launching Cursor:

```bash
export PLUGLAYER_API_KEY="plk_your_token_here"
```

4. Optional if you use a non-default API base:

```bash
export PLUGLAYER_API_URL="https://api.pluglayer.com"
```

5. Restart Cursor or run `Developer: Reload Window`.
6. Open `Settings -> Plugins` and confirm `PlugLayer` appears.

### Local install from this repo

```bash
./install.sh
```

## MCP wiring

Cursor should pick up the MCP config from `mcp.json`:

```json
{
  "pluglayer": {
    "command": "pluglayer-mcp launcher",
    "type": "stdio",
    "args": ["..."]
  }
}
```

## How to use it well

### Good first prompts
- "Inspect this repo and tell me the safest PlugLayer deploy path."
- "Use the PlugLayer deploy agent and ship this repo with the default domain first."
- "Use the PlugLayer CI/CD agent and wire GitHub Actions for the app that is already deployed."
- "Use the PlugLayer fix-deploy agent and tell me why this app is crash-looping."
- "Use the PlugLayer domain agent and help me point my registrar to this app."

### Best experience patterns
- For initial deploys or repo-to-cloud work, use the deploy agent.
- For GitHub Actions setup or broken workflows, use the CI/CD agent.
- For failed deploys, logs, or bad runtime behavior, use the fix-deploy agent.
- For DNS and custom domains, use the domain agent.

When the domain agent explains DNS forms, it should translate PlugLayer's exact DNS names into registrar-friendly host entries when needed, such as `@` for the root domain or `_pluglayer-verify` instead of `_pluglayer-verify.example.com` in GoDaddy-style UIs.

## Included assets
- Rule: `rules/pluglayer-deploy.mdc`
- Skills: `skills/`
- Agents: `agents/`

## CI/CD behavior

The CI/CD flow this plugin steers toward is:
1. build OCI image
2. optionally inject build-time env via `PLUGLAYER_BUILD_ENV_JSON`
3. upload image to PlugLayer
4. redeploy the same `app_id`
5. keep the same slug

The generated GitHub workflow should use the public reusable actions repo:
- `pluglayer/actions/github/build-oci-image@main`
- `pluglayer/actions/github/upload-image-to-pluglayer@main`
- `pluglayer/actions/github/redeploy-pluglayer-app@main`

## Scope

This plugin stays in the end-user surface:
- compute is read-only through MCP
- users can operate their own apps and databases
- admin-only compute/cluster mutation is intentionally not part of this plugin
