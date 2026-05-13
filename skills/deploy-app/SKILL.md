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
3. Check what the user already has in the target project and broader stack:
   - existing apps in the project
   - likely databases already deployed
   - whether frontend and backend already exist separately
   - whether they want to deploy databases too or just reuse an existing connection string
4. Based on that analysis, explain a short step-by-step deployment plan and include a recommended option for any non-obvious choice.
5. Check project availability.
   - If the user names a project, use it.
   - If they do not have one, ask what they want to call it and suggest sensible names.
   - Include `[you choose]` as an option when the user wants the agent to decide.
   - Best practice: separate distinct software systems into separate projects.
6. Always ask for the app name before deployment and suggest names that fit the project, for example:
   - `mongo-db`
   - `api-backend`
   - `web-frontend`
   - `<project>-worker`
   - `[you choose]`
7. Treat app name and PlugLayer slug as separate values.
   - App name is the internal display/identity name in PlugLayer.
   - PlugLayer slug is the public route segment for the default PlugLayer URL.
   - Example: if slug is `api`, the URL can look like `https://api.<project>.<user>.apps.pluglayer.io`.
   - Let the user keep them the same or choose different values.
   - Make it clear they can update both later.
8. Before deployment, ask whether they want:
   - the default PlugLayer subdomain for now
   - one of the existing project domains
   - a new custom domain
   Mention they can switch to a custom domain later.
   Make it explicit that changing the PlugLayer slug is different from adding or updating a custom domain.
8. If they choose a custom domain:
   - run provider detection first
   - confirm the detected provider with the user
   - if detection is uncertain, show several provider options plus `[you choose]` and let them type their own
   - only then show DNS record instructions
9. DNS instructions must always be shown in a markdown table with columns:
   - Type
   - Name / Host
   - Content / Value / Target
   - Description
10. Check compute availability.
   - If sizing is unclear, call `estimate_compute`.
   - If compute is missing or zero, call `estimate_compute`, offer PlugLayer compute, share the compute link, and stop deployment until the user completes that step.
   - After the user says they added or purchased compute, always check available compute again before deploying.
11. Check whether this project already has apps.
   - List the available apps for the user.
   - If one likely matches the intended deploy target, ask whether they want to update it, replace it, or add a separate new app.
   - Include a recommended option and `[you choose]` for complex operations.
   - If the namespace already looks full, quota-limited, or occupied by a failed/crash-looping older workload, refuse the separate new-app path by default and steer the user into update or replace flow instead.
12. Choose placement:
   - `personal` when the user explicitly wants dedicated personal compute
   - `shared` when the user explicitly wants shared PlugLayer compute
   - `auto` otherwise
   - Include `[you choose]` when the user delegates the decision.
13. Decide deploy type:
   - local build + image deploy when the current repo should be shipped now
   - Docker image when an image already exists in an allowed listed repository
   - docker-compose when multiple services should run together
   - public Docker Hub image directly for standard databases when a trusted public image already exists
14. Deploy.
15. Tell the user deployment usually takes around 10 minutes and offer to check status later.
16. If they chose a new custom domain, ask them to reply after they add the records so verification can continue.
17. After deployment, retrieve all apps in the project and suggest useful cross-app env updates such as frontend → backend URL or backend → database connection string changes.

## Required checks before deploy
- project exists
- app name is confirmed
- at least one ready compute path exists for the chosen placement
- port is known
- app binds in a network-safe way
- env vars are accounted for
- Dockerfile and env file are detected, created if needed, and tested locally when the repo is being built locally
- destination registry is one of the listed allowed repositories/registries

## PlugLayer MCP actions to prefer
- get current user
- get user context
- update user_context after completed tasks or important learned preferences
- list/create/remove projects
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
- check slug availability before using or changing a PlugLayer slug
- update the PlugLayer route slug for an app
- get app/database connection env vars and connection strings after provisioning
- add / verify / attach custom domains as a separate operation
- get deployment/app status
- get task status
- get logs when needed
- app terminal when troubleshooting a live app
- remove app when the user explicitly wants to remove it

Compute through MCP is read-only. Do not try to add, archive, remove, or otherwise mutate compute inventory from the plugin/MCP surface. If the user needs more capacity, use estimate + marketplace/shared-compute guidance, or direct an admin to the platform UI for compute administration.
MCP exposes no admin-only functions. Stay within end-user actions only.

## Local repo deploy default
For current-repo deployments:

1. inspect the codebase
2. detect whether a Dockerfile and env file already exist and whether they are correct for production deployment
3. build or improve the Dockerfile so it is:
   - optimized for low size
   - architecture-agnostic
   - reasonably secure
4. understand env vars before build/deploy
   - identify required vars
   - identify vars likely to change after deployment, such as redirect URLs, callback URLs, public API URLs, connection strings, and app slugs
   - update them in advance when safe
   - otherwise ask or clearly inform the user
5. build the image locally
6. test the image locally before pushing it
7. keep temporary deployment files inside `.pluglayer/` in the local repo
8. if the image only exists locally, export it with `docker save` into `.pluglayer/` and use the uploaded-image deploy flow
9. use plain `deploy_image` only when the source image is already pullable from an allowed listed repository
10. after push/deploy, delete:
   - the temporary image archive
   - any other temporary files in `.pluglayer/`
   - the local built image if it is no longer needed

Do not ask the user for a prebuilt image if the current repo can be built confidently.
Do not push images to any repository that is not listed by PlugLayer.

## Database rule
- If the app is a common database and a trusted public Docker Hub image exists, prefer the public image directly.
- Do not mirror or push those public database images unless there is a very specific reason.
- Before deploying a database, ask whether the user wants the database deployed here or already has one elsewhere and only wants to use the connection string.

## Env var rule
- Always check likely environment variables before deploy.
- Remember that PlugLayer slug changes affect default PlugLayer URLs, while custom domain changes are a separate routing concern.
- If some env vars may differ in the deployed version, call that out clearly and confirm with the user.
- Remind the user that env vars can be updated later by asking to update them.
- When only env vars change, explain that the app will restart/redeploy.
- After a successful deploy, look at the other apps in the project and suggest concrete env updates when one app now depends on another.
- For databases, fetch the concrete connection details after provisioning and offer to update dependent backend/frontend apps with those exact values.
- Before assigning or changing a PlugLayer slug, check that it is available in the target project.

## Existing app rule
Before deploying into a project that already has one or more apps:

1. inspect the existing apps in that project
2. show the available apps to the user
3. if one looks like the same app, ask the user which path they want:
   - update the existing app
   - replace the existing app
   - add a separate new app
   - `[you choose]`
4. do not silently create duplicates when the user's intent is ambiguous
5. if quota or compute is tight, explain that replacing or removing the old app may be necessary before the new deploy can succeed
6. if the project namespace is already full and a similar app exists, do not continue with a brand-new app deploy unless the user explicitly insists on adding a second app

## Redeploy rule
Before redeploying an existing app:
1. list the relevant existing apps if there is any ambiguity
2. show the app name
3. ask the user to confirm the exact app name
4. only then redeploy

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

## Removal rule
- For apps, remove them when the user explicitly asks.
- Do not confuse updating the PlugLayer slug with adding/updating a custom domain; those are separate user choices and separate actions.
- Removal should tear down the runtime workload, revoke active routing/slugs, and mark the app as removed in PlugLayer.
- Do not describe app removal as archival in end-user MCP flows.

## User context rule
- Call `update_user_context` after completed tasks and whenever you learn something useful about the user.
- Good examples:
  - preferred project naming style
  - preferred compute placement
  - whether they usually keep databases separate
  - known backend/frontend/database relationships in a project
  - preferred domain/provider patterns

## Result format
Always summarize:
- deploy type used
- project
- app name
- domain choice used for now
- compute placement
- app status
- public URL if any
- follow-up env suggestions if relevant
- next step or fix if needed
