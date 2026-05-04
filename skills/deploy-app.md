# Deploy App to PlugLayer

Use this when deploying a repo, Docker image, or compose stack to PlugLayer.

## Preferred flow
1. Inspect the repo.
2. Use an existing project or create one.
3. Ask whether to use the default PlugLayer domain or a custom domain now.
4. Check compute. Estimate first if sizing is unclear.
5. Prefer PlugLayer managed compute wording over SSH wording.
6. If deploying the current repo, build locally and deploy the built image.
7. Queue deploy and tell the user it usually takes around 10 minutes.
8. Offer to check status later.
