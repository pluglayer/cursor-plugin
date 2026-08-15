# Changelog

## 1.1.17
- Keep the local stdio MCP server discoverable before authentication and apply saved token/API URL changes on the next tool call without a reload.
- Clarify that Cursor's generic `mcp_auth` action does not configure local stdio credentials.
- Warn when a manual global or project PlugLayer MCP entry duplicates the plugin server.

## 1.1.16
- Keep the bundled v1 FastMCP runtime below the breaking MCP Python SDK v2 release.
- Refresh `pluglayer-mcp` when Cursor starts it and avoid loading unrelated login-shell startup files.

## 1.1.15
- Treat root and `www` as separate routes, require an explicit root redirect or attachment, and validate nested-path preservation.

## 1.1.14
- Teach agents, rules, and deployment guidance to use `rename_project` for display-name-only project renames.

## 1.1.13
- Keep the packaged MCP server synchronized with token-only refreshes by resolving the saved credential file at request time.
- Fail closed with actionable setup guidance instead of starting with an empty bearer token.

## 1.1.12
- Prevent GoDaddy apex CNAME instructions and guide users to a PlugLayer `www` domain plus GoDaddy HTTPS 301 forwarding.

## 1.1.11
- Add secure arbitrary env import through MCP and document JSON, dotenv/config content, and reusable Action flows without returning secret values.

## 1.1.9
- Added `submit_feedback`, feedback history/status guidance, a focused feedback agent and skill, and an always-applied safe automatic-feedback rule.

## 1.1.4
- Added the shared 500-line maximum for Python source files across plugin and MCP development.

## 1.1.3
- `fix-deploy` skill and agent now classify transient recreate-rollout noise (the previous pod transiently `Failed` while the new pod is still `Pending`) and steer toward a safe retry instead of app changes
- `setup-cicd` skill and agent now carry a reusable-actions reference (`build-oci-image`, `upload-image-to-pluglayer`, `redeploy-pluglayer-app`, `apply-env-and-restart`), forbid hand-rolled polling/retry loops, and require a per-app `concurrency` group; the redeploy and env-apply actions wait for their task and surface real rollout failure detail

## 1.0.0
- Initial Cursor plugin package aligned with Cursor's plugin template shape
- Added PlugLayer MCP wiring, deployment rule, and deployment/domain/debugging skills
