---
name: setup-cicd
description: Set up GitHub-based CI/CD for an already deployed PlugLayer app by checking that the local repo has git plus a GitHub origin, generating the PlugLayer workflow for the target app, writing it into .github/workflows, and telling the user which secret to add.
---

# Set Up GitHub CI/CD for PlugLayer

Use this skill when the user wants GitHub Actions or CI/CD for an app that is already deployed on PlugLayer.

## What this skill assumes
- The app already exists in PlugLayer.
- The repo either already has git + a GitHub `origin`, or the user can provide `owner/repo`.
- The deployment should keep the same PlugLayer app identity and slug on future deploys.

## Workflow
1. Confirm or discover the target PlugLayer app:
   - if the app id is not obvious, list project apps first
   - confirm the exact existing app to wire into CI/CD
2. Check local repo readiness with:
   - `python3 pluglayer-cursor-plugin/skills/setup-cicd/scripts/detect_github_repo.py`
3. If the script reports a valid GitHub repo/origin:
   - use that `repo_slug`
   - otherwise ask for `owner/repo`
4. Call the PlugLayer MCP CI/CD tool:
   - `generate_github_actions(project_id=..., app_id=..., repo=...)`
5. Write the returned YAML to:
   - `.github/workflows/deploy-pluglayer.yml`
6. Tell the user to add this GitHub secret:
   - `PLUGLAYER_API_KEY`
7. Explain what the pipeline does:
   - builds a multi-arch OCI archive
   - uploads it to PlugLayer
   - redeploys the same app id
   - keeps the existing slug unchanged

## Notes
- Do not create a brand-new PlugLayer app for CI/CD setup.
- Do not change the slug unless the user explicitly asks.
- Prefer this skill after the initial deploy already succeeded.
- If the user changed code, CI/CD should roll forward with a new image tag, not a plain restart.
