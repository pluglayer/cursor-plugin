# Changelog

## 1.1.42 — 2026-10-16 (unreleased)

- Direct install starts native sign-in only when a terminal can be opened. Headless runs keep the manual connector step.

## 1.1.41 — 2026-10-15 (unreleased)

- Route PlugLayer questions to the matching tool. Use setup only for install, update, or reconnect, and do not browse the site for data the tools return.
- Direct install opens native PlugLayer sign-in when the agent CLI provides `mcp login`, instead of leaving connector authentication as a separate click.

## 1.1.40 — 2026-10-15 (unreleased)

- Use documented MCP/hook paths, wrapped MCP configuration and a bundled logo.
- Verify setup and native component discovery; keep focused agents available without changing unrelated conversations.
- Document masked connection values, compact template discovery and launch-session cleanup.
- Align version/changelog gates and include marketplace readiness guidance.

## 1.1.39

- Fix public plugin loading, registration and OAuth recovery (2026-10-14, `1478bac`).

## 1.1.38

- Update PlugLayer plugin to version 1.1.38, removing unnecessary OAuth prompts from installation scripts. Enhance post-installation user guidance by clarifying connection through the native plugin UI and informing users about necessary actions after installation. Revise installation documentation to reflect these changes, improving overall user experience. (2026-10-14, `b1dc184`).

## 1.1.37

- Update installation scripts and documentation for PlugLayer Cursor plugin to version 1.1.37. Refine OAuth connection prompts in Bash and PowerShell installers for improved user experience. Enhance terminal interaction handling during installation and update installation instructions to reflect the new version. (2026-10-12, `e5cbb83`).

## 1.1.36

- Add installation scripts for PlugLayer Cursor plugin in Bash and PowerShell. Update installation instructions and README to clarify setup and capabilities. Bump plugin version to 1.1.36 and enhance plugin description to reflect new features and improvements. (2026-10-12, `5cd66b4`).

## 1.1.35

- No distinct version declaration exists in this package’s checked-in history; this number is not asserted to have shipped.

## 1.1.34

- Bump public plugin release to 1.1.34 (2026-10-11, `345bcac`).

## 1.1.33

- Update version to 1.1.33 in plugin.json for the pluglayer-cursor-plugin (2026-10-11, `e08f50f`).

## 1.1.32

- No distinct version declaration exists in this package’s checked-in history; this number is not asserted to have shipped.

## 1.1.31

- Publish per-agent installation instructions (2026-10-11, `03030f9`).

## 1.1.30

- Release OAuth-compatible public plugin packs (2026-10-11, `8e5bf30`).

## 1.1.29

- Use OAuth hosted MCP for public plugins (2026-10-10, `c841643`).

## 1.1.28

- Add self-contained public plugin agent setup and native validation (2026-10-07, `41959bb`).

## Earlier recorded history

## 1.1.27

- Proactively submit safe PlugLayer feedback for product-caused poor experiences, challenges, friction, confusing guidance, and bugs.

## 1.1.26

- Isolate the public Windows installer from all private Admin, Security Operations, and GTM product details.

## 1.1.25

- Keep submitted feedback unchanged; create new follow-up tickets referencing the previous ticket, including recurrence after resolution.

## 1.1.24

- Plan dedicated compute per app, reuse project capacity first, prevent undersized machine bundles, and add confirmation-gated Extra Compute Requests.

## 1.1.23

- Use the canonical PlugLayer plugin icon and add cached, consent-gated plugin update guidance through PlugLayer MCP.

## 1.1.22

- Add app security checks, IP allowlist and rate-limit skills, and focused security routing.

## 1.1.20
- Support non-interactive quick setup from the portal's short-lived installer exchange.

## 1.1.19
- Add project display-name/description updates through `update_project_metadata`, including explicit description clearing and routing-identity safeguards.

## 1.1.18
- Let users inspect feedback status and update an owned ticket's title/description, while keeping status and resolution admin-managed.
- Check recent feedback and consolidate matching reports instead of creating repetitive tickets.

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
