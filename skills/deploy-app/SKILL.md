---
name: deploy-app
description: Deploy a local repo, Docker image, or docker-compose app to the user's infrastructure on PlugLayer using the PlugLayer MCP tools.
---

# Deploy an App to the User's Infrastructure

Use this skill when the user wants to ship an app through PlugLayer.

## Conversation flow
1. Inspect the repo or artifact first.
2. Analyze the app shape before proposing deployment:
   - frontend
   - backend/API
   - workers / queues
   - databases / caches
   - storage / volumes
   - third-party services
3. Based on that analysis, explain a short step-by-step deployment plan.
4. Check project availability.
   - If the user names a project, use it.
   - If they do not have one, ask what they want to call it and suggest sensible names.
   - Include `[you choose]` as an option when the user wants the agent to decide.
   - Best practice: separate distinct software systems into separate projects.
5. Always ask for the app name before deployment and suggest names that fit the project, for example:
   - `mongo-db`
   - `api-backend`
   - `web-frontend`
   - `<project>-worker`
   - `[you choose]`
6. Before deployment, ask whether they want:
   - the default PlugLayer subdomain for now
   - one of the existing project domains
   - a new custom domain
   Mention they can switch to a custom domain later.
7. If they choose a custom domain:
   - run provider detection first
   - confirm the detected provider with the user
   - if detection is uncertain, show several provider options plus `[you choose]` and let them type their own
   - only then show DNS record instructions
8. DNS instructions must always be shown in a markdown table with columns:
   - Type
   - Name / Host
   - Content / Value / Target
   - Description
9. Check compute availability.
   - If sizing is unclear, call `estimate_compute`.
   - If compute is missing or zero, call `estimate_compute`, offer PlugLayer compute, share the compute link, and stop deployment until the user completes that step.
   - After the user says they added or purchased compute, always check available compute again before deploying.
10. Check whether this project already has apps.
   - If it likely already contains the same app, ask whether they want to update the existing app, replace it, or add a separate new app.
   - If they want to update it, prefer redeploying/updating the existing app instead of creating a duplicate.
   - If they want to replace it, explain that the older app may need to be removed to free quota and avoid confusion.
   - If the namespace already looks full, quota-limited, or occupied by a failed/crash-looping older workload, refuse the separate new-app path by default and steer the user into update or replace flow instead.
11. Choose placement:
   - `personal` when the user explicitly wants dedicated personal compute
   - `shared` when the user explicitly wants shared PlugLayer compute
   - `auto` otherwise
   - Include `[you choose]` when the user delegates the decision.
12. Decide deploy type:
   - local build + image deploy when the current repo should be shipped now
   - Docker image when an image already exists in an allowed listed repository
   - docker-compose when multiple services should run together
13. Deploy.
14. Tell the user deployment usually takes around 10 minutes and offer to check status later.
15. If they chose a new custom domain, ask them to reply after they add the records so verification can continue.

## Required checks before deploy
- project exists
- app name is confirmed
- at least one ready compute path exists for the chosen placement
- port is known
- app binds in a network-safe way
- env vars are accounted for
- destination registry is one of the listed allowed repositories/registries

## PlugLayer MCP actions to prefer
- get current user
- get user context
- update user context
- list/create projects
- get project
- get apps by project
- list project domains
- get domains by project
- detect custom domain provider
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
- app terminal when troubleshooting a live app

## Local repo deploy default
For current-repo deployments:

1. inspect the codebase
2. build or improve the Dockerfile so it is:
   - optimized for low size
   - architecture-agnostic
   - reasonably secure
3. build the image locally
4. test the image locally before pushing it
5. keep temporary deployment files inside `.pluglayer/` in the local repo
6. if the image only exists locally, export it with `docker save` into `.pluglayer/` and use the uploaded-image deploy flow
7. use plain `deploy_image` only when the source image is already pullable from an allowed listed repository
8. after push/deploy, delete:
   - the temporary image archive
   - any other temporary files in `.pluglayer/`
   - the local built image if it is no longer needed

Do not ask the user for a prebuilt image if the current repo can be built confidently.
Do not push images to any repository that is not listed by PlugLayer.

## Env var rule
- Always check likely environment variables before deploy.
- If some env vars may differ in the deployed version, call that out clearly and confirm with the user.
- Remind the user that env vars can be updated later by asking to update them.
- When only env vars change, explain that the app will restart/redeploy.

## Existing app rule
Before deploying into a project that already has one or more apps:

1. inspect the existing apps in that project
2. if one looks like the same app, ask the user which path they want:
   - update the existing app
   - replace the existing app
   - add a separate new app
   - `[you choose]`
3. do not silently create duplicates when the user's intent is ambiguous
4. if quota or compute is tight, explain that replacing or deleting the old app may be necessary before the new deploy can succeed
5. if the project namespace is already full and a similar app exists, do not continue with a brand-new app deploy unless the user explicitly insists on adding a second app

## Redeploy rule
Before redeploying an existing app:
1. show the app name
2. ask the user to confirm the exact app name
3. only then redeploy

## Domain guidance
When explaining DNS records:
- say `Name / Host`
- say `Content / Value`
- say `Target` for CNAME when needed
- mention that some providers use `@` for the root domain
- mention that root and `www` sometimes behave differently
- tailor click-path notes to the confirmed provider when possible

After showing DNS instructions, tell the user:
- "After you add the records, tell me you've added them and I'll verify and continue."

## Wording rule
Prefer ownership wording such as:
- your cloud
- your infrastructure
- your app runtime
- your compute

Avoid phrasing like:
- deploy your app to PlugLayer
when what you really mean is deploying onto the user's own compute or assigned infrastructure through PlugLayer.

## Result format
Always summarize:
- deploy type used
- project
- app name
- domain choice used for now
- compute placement
- app status
- public URL if any
- next step or fix if needed
