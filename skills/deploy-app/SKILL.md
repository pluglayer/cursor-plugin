---
name: deploy-app
description: Deploy a local repo, Docker image, or docker-compose app to PlugLayer using the PlugLayer MCP tools.
---

# Deploy App to PlugLayer

Use this skill when the user wants to ship an app to PlugLayer.

## Conversation flow
1. Inspect the repo or artifact first.
2. Check project availability.
   - If the user names a project, use it.
   - If they do not have one, say that clearly, ask what they want to call it, and create it.
3. Before deployment, ask whether they want:
   - the default PlugLayer domain for now
   - their own custom domain now
   Mention they can change it later.
4. Check compute availability.
   - If sizing is unclear, call `estimate_compute`.
   - If compute is missing, prefer PlugLayer compute marketplace language over SSH wording.
5. Choose placement:
   - `personal` when the user explicitly wants dedicated personal compute
   - `shared` when the user explicitly wants shared PlugLayer compute
   - `auto` otherwise
6. Decide deploy type:
   - local build + image deploy when the current repo should be shipped now
   - Docker image when an image already exists
   - docker-compose when multiple services should run together
7. Deploy.
8. Tell the user deployment usually takes around 10 minutes and offer to check status later.
9. If they chose a custom domain, walk them through DNS and ask them to reply after they add the records.

## Required checks before deploy
- project exists
- at least one ready compute path exists for the chosen placement
- port is known
- app binds in a network-safe way
- env vars are accounted for

## PlugLayer MCP actions to prefer
- list/create projects
- estimate compute when sizing is unclear
- list PlugLayer compute options
- get my available compute
- get compute summary
- list registries
- deploy image
- deploy compose
- update app domain
- add / verify / attach custom domains
- get deployment/app status
- get task status
- get logs when needed

## Local repo deploy default
For current-repo deployments:

1. inspect the codebase
2. build the image locally
3. tag it clearly
4. deploy it through PlugLayer's image flow
5. prefer PlugLayer-managed mirroring when available

Do not ask the user for a prebuilt image if the current repo can be built confidently.

## Domain guidance
When explaining DNS records:
- say `Name / Host`
- say `Content / Value`
- say `Target` for CNAME when needed
- mention that some providers use `@` for the root domain
- mention that root and `www` sometimes behave differently

After showing DNS instructions, tell the user:
- "After you add the records, tell me you've added them and I'll verify and continue."

## Result format
Always summarize:
- deploy type used
- project
- domain choice used for now
- compute placement
- app status
- public URL if any
- next step or fix if needed
