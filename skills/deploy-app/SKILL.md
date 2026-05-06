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
3. Before deployment, list the domains already available in that project.
4. When asking which domain path they want now, include:
   - any existing project domain that is already attached or verified
   - the default PlugLayer domain
   - adding a new custom domain
   Mention they can change it later.
5. Check compute availability.
   - If sizing is unclear, call `estimate_compute`.
   - If compute is missing or zero, call `estimate_compute`, offer PlugLayer compute, share the PlugLayer link for getting or purchasing compute, and do not deploy yet.
   - After the user says they added or purchased compute, always check available compute again before deploying.
6. Check whether this project already has apps.
   - If it likely already contains the same app, ask the user whether they want to update the existing app, replace it, or add a separate new app.
   - If they want to update it, prefer redeploying/updating the existing app instead of creating a duplicate.
   - If they want to replace it, explain that the older app may need to be removed to free quota and avoid confusion.
7. Choose placement:
   - `personal` when the user explicitly wants dedicated personal compute
   - `shared` when the user explicitly wants shared PlugLayer compute
   - `auto` otherwise
8. Decide deploy type:
   - local build + image deploy when the current repo should be shipped now
   - Docker image when an image already exists
   - docker-compose when multiple services should run together
9. Deploy.
10. Tell the user deployment usually takes around 10 minutes and offer to check status later.
11. If they chose a new custom domain, walk them through DNS and ask them to reply after they add the records.

## Required checks before deploy
- project exists
- at least one ready compute path exists for the chosen placement
- port is known
- app binds in a network-safe way
- env vars are accounted for

## PlugLayer MCP actions to prefer
- list/create projects
- get project
- get apps by project
- list project domains
- get domains by project
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

## Existing app rule
Before deploying into a project that already has one or more apps:

1. inspect the existing apps in that project
2. if one looks like the same app, ask the user which path they want:
   - update the existing app
   - replace the existing app
   - add a separate new app
3. do not silently create duplicates when the user's intent is ambiguous
4. if quota or compute is tight, explain that replacing or deleting the old app may be necessary before the new deploy can succeed

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
