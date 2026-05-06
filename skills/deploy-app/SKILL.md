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
   - If compute is missing or zero, call `estimate_compute`, share the PlugLayer link for getting or purchasing compute, and do not deploy yet.
   - After the user says they added or purchased compute, always check available compute again before deploying.
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
- upload image archive and deploy
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
4. if the image only exists locally, export it with `docker save` and use the uploaded-image deploy flow
5. use plain `deploy_image` only when the source image is already pullable from a registry
6. prefer PlugLayer-managed mirroring when available

Do not ask the user for a prebuilt image if the current repo can be built confidently.

## Missing compute rule
If available compute is zero or insufficient:

1. stop before deployment
2. run `estimate_compute`
3. share the returned PlugLayer compute link
4. wait for the user to complete the compute step
5. check available compute again
6. only then deploy

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
