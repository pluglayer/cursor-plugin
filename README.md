# PlugLayer Cursor Plugin

PlugLayer's Cursor plugin bundles:
- the PlugLayer MCP server config
- a PlugLayer deployment rule for Cursor
- practical skills for deploy, domain setup, repo inspection, and deploy troubleshooting

It is packaged in Cursor's plugin layout with:
- `.cursor-plugin/plugin.json`
- `mcp.json`
- `rules/*.mdc`
- `skills/*/SKILL.md`

This structure is aligned with Cursor's plugin docs and plugin template:
- [Cursor plugins docs](https://cursor.com/docs/plugins#creating-plugins)
- [Cursor plugin template](https://github.com/cursor/plugin-template)

## Requirements
1. Cursor with plugin support
2. `uvx` available on your machine
3. A PlugLayer API token from the PlugLayer Settings page
4. Environment variable set before launching Cursor:

```bash
export PLUGLAYER_API_KEY="plk_your_token_here"
```

## Install step by step

### Option 1: Test locally from your machine
1. Clone or copy this plugin folder.
2. Make sure this file exists at the plugin root:
   - `.cursor-plugin/plugin.json`
3. Create Cursor's local plugin directory if needed:

```bash
mkdir -p ~/.cursor/plugins/local
```

4. Copy this plugin into Cursor's local plugins folder:

```bash
cp -R pluglayer-cursor-plugin ~/.cursor/plugins/local/pluglayer-cursor-plugin
```

5. Export your PlugLayer API token in the shell you use to launch Cursor:

```bash
export PLUGLAYER_API_KEY="plk_your_token_here"
```

6. Restart Cursor or run `Developer: Reload Window`.
7. Open `Settings -> Plugins` and confirm `PlugLayer` appears in the installed plugin list.

### Option 2: Prepare for marketplace/team distribution
1. Keep the folder structure exactly as shipped here.
2. Publish or sync this plugin repo to your public/plugin distribution target.
3. Install it through Cursor's plugin flow when your marketplace distribution is ready.

## What Cursor will pick up
- MCP config from [`mcp.json`](/Users/erfan/Documents/develop/claude-code/pluglayer/pluglayer-cursor-plugin/mcp.json)
- Deployment rule from [`rules/pluglayer-deploy.mdc`](/Users/erfan/Documents/develop/claude-code/pluglayer/pluglayer-cursor-plugin/rules/pluglayer-deploy.mdc)
- Skills from [`skills/`](/Users/erfan/Documents/develop/claude-code/pluglayer/pluglayer-cursor-plugin/skills)

## PlugLayer MCP configuration

```json
{
  "pluglayer": {
    "command": "uvx",
    "type": "stdio",
    "args": ["pluglayer-mcp"],
    "env": {
      "PLUGLAYER_API_KEY": "${env:PLUGLAYER_API_KEY}"
    }
  }
}
```

## Good first prompts
- "Inspect this repo and tell me the safest PlugLayer deploy path."
- "Build this repo, deploy it to PlugLayer, and use the default domain for now."
- "Help me attach my custom domain and explain exactly what to put in my DNS provider."
- "Why did this PlugLayer deploy fail? Check logs and fix it."

## Scope
This plugin does not expose PlugLayer admin-only tools. Compute stays read-only through MCP, users can remove their own apps, and project removal remains an end-user project workflow rather than an admin action.
