# PlugLayer Cursor Plugin

This bundle connects Cursor to PlugLayer using the published `pluglayer-mcp` package and a Cursor rule focused on fast end-user deployments.

## What it includes
- PlugLayer MCP wiring via `.mcp.json`
- Cursor rule at `.cursor/rules/pluglayer-deploy.mdc`
- Deployment, repo-inspection, domain, and troubleshooting guidance in `skills/`

## Requirements
1. `pluglayer-mcp` must be available through `uvx`
2. You need a PlugLayer API token from the PlugLayer Settings page
3. Export the token before launching Cursor:

```bash
export PLUGLAYER_API_KEY="plk_your_token_here"
```

## MCP configuration

```json
{
  "pluglayer": {
    "command": "uvx",
    "type": "stdio",
    "args": ["pluglayer-mcp"],
    "env": {
      "PLUGLAYER_API_KEY": "${PLUGLAYER_API_KEY}"
    }
  }
}
```

## Suggested setup
1. Copy `.mcp.json` into your Cursor MCP configuration flow.
2. Copy `.cursor/rules/pluglayer-deploy.mdc` into your project if you want Cursor to bias toward the PlugLayer deploy workflow.
3. Keep the `skills/` docs nearby for human-readable operator guidance.

## Good first prompts
- "Inspect this repo and tell me the safest PlugLayer deploy path."
- "Build this repo, deploy it to PlugLayer, and use the default domain for now."
- "Help me attach my custom domain and explain exactly what to put in my DNS provider."
- "Why did this PlugLayer deploy fail? Check logs and fix it."
